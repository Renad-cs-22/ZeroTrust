# ZeroTrust
A Risk-Based Conditional Access Engine simulating Microsoft Entra ID (Azure AD) policies and emitting SIEM-ready JSON logs.
# 🛡️ Zero Trust Adaptive Access Engine

An enterprise-grade Python simulation of a **Risk-Based Conditional Access Gate** modeled after **Microsoft Entra ID (Azure AD)** and **Okta Adaptive MFA** architectures. This project processes authentication requests dynamically using contextual telemetry and outputs structured JSON logs designed for direct ingestion into **SIEM platforms** (Microsoft Sentinel / Splunk).

---

## 🌟 Key Capabilities & Security Concepts

* **Zero Trust Architecture (ZTA):** Enforces *Never Trust, Always Verify* principles by evaluating risk dynamically on every access attempt.
* **Context-Aware Risk Engine:** Evaluates contextual risk factors including geographic anomalies and off-hours request timeframes.
* **Adaptive Step-Up MFA:** Automatically triggers MFA challenges for medium-risk scenarios ($30\% \le \text{Risk} < 80\%$).
* **Automated Policy Enforcement:** Instantly blocks high-risk access attempts ($\text{Risk} \ge 80\%$) based on policy constraints.
* **SIEM-Ready JSON Telemetry:** Emits structured, machine-readable JSON logs following Enterprise SIEM standards.
* **Cryptographic Best Practices:** Uses Python's `secrets` module for secure OTP generation and constant-time string comparison (`secrets.compare_digest`) to protect against timing attacks.

---

## 📐 Architecture & Logic Flow

| Risk Level | Score Threshold | Policy Applied | Action Taken |
| :--- | :--- | :--- | :--- |
| **Low Risk** | $< 30\%$ | `GrantAccessLowRisk` | Direct Access Granted |
| **Medium Risk** | $30\% \le \text{Score} < 80\%$ | `RequireMFA_MediumRisk` | Challenge Required (Adaptive MFA) |
| **High Risk** | $\ge 80\%$ | `DenyAccessHighRisk` | Automatic Access Block |

---

## 💻 Sample Output (SIEM Log Telemetry)

```json
{
  "timestamp": "2026-10-03T06:58:43.040769",
  "event_type": "CONDITIONAL_ACCESS_EVALUATION",
  "status": "BLOCKED",
  "system_component": "ZeroTrust_Adaptive_Gate",
  "details": {
    "user": "renad_admin",
    "risk_score": 80,
    "risk_factors": ["GeographicAnomaly(US)", "UnusualTimeWindow(1:00)"],
    "enforced_policy": "DenyAccessHighRisk"
  }
}
