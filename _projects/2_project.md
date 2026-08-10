---
layout: page
title: Production MLOps Fraud Detection
description: End-to-end ML pipeline with experiment tracking, drift monitoring, feature store, and CI/CD — deployed on Render
img:
importance: 1
category: work
---

A production-grade fraud detection pipeline built on a severely imbalanced dataset (577:1 fraud ratio), with full MLOps infrastructure. Live at [production-fraud-mlops.onrender.com/docs](https://production-fraud-mlops.onrender.com/docs).

## Why PR-AUC, Not Accuracy

On a 577:1 imbalanced dataset, a model that predicts "not fraud" for every transaction scores 99.8% accuracy. Useless. I chose PR-AUC as the primary metric because it directly measures performance on the minority class. The final XGBoost model achieved **PR-AUC = 0.8648** after Optuna hyperparameter search (50 trials).

## Architecture

- **Experiment tracking:** 50+ runs logged to MLflow — parameters, metrics, SHAP artifacts, model binaries
- **Model:** XGBoost, tuned via Optuna with stratified train/val/test split
- **Interpretability:** SHAP TreeExplainer on top predictions — force plots and summary plots saved as MLflow artifacts
- **Drift monitoring:** Evidently AI reports tracking covariate, concept, and label drift
- **Feature store:** Feast configured for offline→online feature serving, wired into the FastAPI prediction endpoint
- **Serving:** FastAPI with /predict and /predict/batch endpoints, Pydantic validation, prediction logging
- **Deployment:** Dockerized (multi-stage build, <500MB), deployed to Render with GitHub Actions CI/CD

## Key Results

- **PR-AUC = 0.8648** (XGBoost, Optuna-tuned)
- **50+ experiment runs** tracked in MLflow with full reproducibility
- **Drift detection** correctly identifies distribution shifts in simulated traffic
- **CI/CD green** on every push to main

## Links

- **Live API:** [production-fraud-mlops.onrender.com/docs](https://production-fraud-mlops.onrender.com/docs)
- **GitHub:** [github.com/esther-jk7/production-fraud-mlops](https://github.com/esther-jk7/production-fraud-mlops)
