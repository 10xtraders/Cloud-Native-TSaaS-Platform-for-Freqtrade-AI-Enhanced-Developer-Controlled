# deploy_bot_api.py

"""
Minimal Python Flask API for deploying Freqtrade bots using the DerSalvador freqtrade-k8s-helm-chart.

Core Features:
- Receives bot deployment requests as JSON POSTs.
- Expects a Freqtrade-compatible config dict, strategy class name, and Python code.
- Builds Helm values.yaml dynamically and applies via Helm+kubectl.
- Ensures Kubernetes namespace and ingress creation.

Usage:
1. POST a JSON payload (see SAMPLE below) to /deploy-bot.
2. Your config can be generated in-app, by AI, or copied from Freqtrade sample configs.
3. The backend will deploy your bot instantly in Kubernetes.

PRODUCTION TIP: Extend with authentication, bot status endpoints, and logging as needed.
"""

import os
import subprocess
import yaml
import logging
from flask import Flask, request, jsonify
from kubernetes import client, config as k8s_config
from kubernetes.client.rest import ApiException

# Initialize Flask app
app = Flask(__name__)
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# Load Kubernetes config (tries in-cluster, falls back to local kubeconfig)
try:
    k8s_config.load_incluster_config()
    logger.info("Loaded in-cluster Kubernetes config")
except Exception:
    k8s_config.load_kube_config()
    logger.info("Loaded kubeconfig for local development")
k8s = client.CoreV1Api()

# Paths for Helm chart and ingress domain (adjust for your deployment)
HELM_CHART_PATH = os.environ.get("HELM_CHART", "/home/ubuntu/freqtrade-k8s-helm-chart")
BASE_DOMAIN = os.environ.get("BASE_DOMAIN", "10xtraders.ai")

def build_user_values(namespace, strategy_class, strategy_code, config_data):
    """
    Build the minimal values.yaml structure for freqtrade-k8s-helm-chart.

    :param namespace: Namespace to deploy bot into (should be unique per bot)
    :param strategy_class: Python class name for the trading strategy
    :param strategy_code: Python code of the strategy
    :param config_data: User's Freqtrade config dict
    :return: Dict to be rendered as values.yaml
    """
    return {
        "config": config_data,
        "strategies": {f"{strategy_class}.py": strategy_code},
        "bot": {
            "strategy_name": strategy_class,
            "args": [
                "trade", "--rpc-enabled",
                "--config", f"/freqtrade/config/config-{namespace}.json",
                "--strategy", strategy_class
            ],
            "resources": {
                "requests": {"cpu": "500m", "memory": "1Gi"},
                "limits": {"cpu": "1", "memory": "2Gi"}
            }
        },
        "ingress": {
            "enabled": True,
            "host": BASE_DOMAIN,
            "paths": [
                {
                    "path": f"/user/{namespace}(/|$)(.*)",
                    "pathType": "Prefix",
                    "backend": {
                        "service": {"name": f"freqtrade-{namespace}", "port": {"number": 8084}}
                    }
                }
            ]
        }
    }

@app.route('/deploy-bot', methods=['POST'])
def deploy_bot():
    """
    POST endpoint to deploy a Freqtrade bot to Kubernetes using the Helm chart.

    Expected JSON payload:
    {
      "namespace": "myuniquebotnamespace",
      "strategy_class": "MyStrategy",
      "strategy_code": "class MyStrategy(...): ...",
      "config": { ... }  # Freqtrade config dict
    }

    See Freqtrade sample config on my README or use samples from: https://www.freqtrade.io/en/stable/configuration/
    """
    data = request.json
    namespace = data.get("namespace")
    strategy_class = data.get("strategy_class")
    strategy_code = data.get("strategy_code")
    config_data = data.get("config", {})

    # Validate required fields
    if not all([namespace, strategy_class, strategy_code]):
        return jsonify({"error": "Missing required fields (namespace, strategy_class, strategy_code)"}), 400

    # Ensure Kubernetes namespace exists
    ns_body = client.V1Namespace(metadata=client.V1ObjectMeta(name=namespace))
    try:
        k8s.create_namespace(ns_body)
        logger.info(f"Created namespace: {namespace}")
    except ApiException as e:
        if e.status != 409:  # 409 = AlreadyExists
            logger.error(f"Failed to create namespace: {e}")
            return jsonify({"error": f"Namespace creation failed: {e}"}), 500
        logger.info(f"Namespace already exists: {namespace}")

    # Build values.yaml for Helm chart
    user_values = build_user_values(namespace, strategy_class, strategy_code, config_data)
    user_values_yaml = yaml.safe_dump(user_values)

    # Helm template rendering
    helm_cmd = [
        "helm", "template", "--values", "-", "-n", namespace, HELM_CHART_PATH
    ]
    rendered = subprocess.run(helm_cmd, input=user_values_yaml, capture_output=True, text=True)
    if rendered.returncode != 0:
        logger.error(f"Helm template failed: {rendered.stderr}")
        return jsonify({"error": "Helm template failed", "details": rendered.stderr}), 500

    # kubectl apply the rendered manifest
    apply = subprocess.run(["kubectl", "apply", "-n", namespace, "-f", "-"],
                           input=rendered.stdout, capture_output=True, text=True)
    if apply.returncode != 0:
        logger.error(f"Kubernetes apply failed: {apply.stderr}")
        return jsonify({"error": "Kubernetes apply failed", "details": apply.stderr}), 500

    logger.info(f"Bot deployment initiated for {namespace}")
    return jsonify({"status": "deploying", "namespace": namespace, "message": "Bot deployment initiated."}), 200

if __name__ == "__main__":
    # Example: Run with `python deploy_bot_api.py`
    app.run(host="0.0.0.0", port=5002)

"""
---------------------------
EXAMPLE: How to call /deploy-bot with sample config
---------------------------

POST /deploy-bot
Content-Type: application/json

{
  "namespace": "simplersistrategy1747867348688vgueu8",
  "strategy_class": "Simplersistrategy1747867348688vgueu8",
  "strategy_code": "class Simplersistrategy1747867348688vgueu8(...): ...",   # Your strategy code as a string
  "config": {
    // Paste your Freqtrade config here or edit my sample config from my README.  
    // You can use AI-generated configs, or copy-paste from: https://www.freqtrade.io/en/stable/configuration/
  }
}

---------------------------
"""
