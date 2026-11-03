import streamlit as st

# Set widescreen page layout and title
st.set_page_config(page_title="Ag Lending Simulator", layout="wide", page_icon="🚜")

# Custom CSS for styling
st.markdown("""
    <style>
    .main-title { font-size: 32px; font-weight: bold; color: #1A252C; text-align: center; margin-bottom: 5px; }
    .subtitle { font-size: 16px; color: #5A6A75; text-align: center; margin-bottom: 30px; font-style: italic; }
    .metric-card { background-color: #F8F9FA; padding: 20px; border-radius: 10px; border-left: 5px solid #3B71CA; margin-bottom: 15px; }
    .verdict-box { padding: 25px; border-radius: 12px; text-align: center; font-size: 22px; font-weight: bold; margin-top: 20px; }
    </style>
""", unsafe_allow_html=True)

st.markdown('<div class="main-title">🚜 Agricultural Loan Repayment Capacity Simulator</div>', unsafe_allow_html=True)
st.markdown('<div class="subtitle">Case Study: Prairie View Farms $250,000 Irrigation-Equipment Loan Appraisal</div>', unsafe_allow_html=True)

# -----------------------------------------------------------------------------
# SIDEBAR CONTROL PANEL (Interactive Slide Controls)
# -----------------------------------------------------------------------------
st.sidebar.header("🕹️ Credit Underwriting Controls")
st.sidebar.markdown("---")

st.sidebar.subheader("🔹 1. Cash Inflows")
net_farm_income = st.sidebar.slider("Net Farm Income ($)", min_value=50000, max_value=300000, value=180000, step=5000)
depreciation = st.sidebar.slider("Depreciation (Non-Cash) ($)", min_value=10000, max_value=100000, value=45000, step=500)
non_farm_income = st.sidebar.slider("Non-Farm Income ($)", min_value=0, max_value=50000, value=10000, step=1000)

st.sidebar.subheader("🔸 2. Cash Outflows")
family_living = st.sidebar.slider("Family Living Expenses ($)", min_value=20000, max_value=120000, value=50000, step=1000)
income_taxes = st.sidebar.slider("Estimated Income Taxes ($)", min_value=0, max_value=40000, value=15000, step=500)

st.sidebar.subheader("💼 3. Scheduled Debt Service")
proposed_p_i = st.sidebar.slider("New Irrigation Loan P&I ($)", min_value=10000, max_value=100000, value=58000, step=1000)
existing_debt = st.sidebar.slider("Existing Farm Debt Payments ($)", min_value=10000, max_value=150000, value=72000, step=1000)

# Quick Preset Buttons for the Lecture
st.sidebar.subheader("🎯 Scenario Quick Presets")
col_pre1, col_pre2 = st.sidebar.columns(2)
if col_pre1.button("☀️ Base Case (Normal Year)"):
    st.rerun()  # Resets to default initial values
if col_pre2.button("🍂 Stress Case (Drought)"):
    # Triggers localized programmatic parameter change warnings in class if manual sliding is too slow
    st.sidebar.info("Manually adjust sliders to mimic the Drought Case: Drop Net Income to $126k & Move Family Living to $42k!")

# -----------------------------------------------------------------------------
# MAIN APP BODY: MATH ENGINE
# -----------------------------------------------------------------------------
# Step 1: Calculate CADS
cads = net_farm_income + depreciation + non_farm_income - family_living - income_taxes

# Step 2: Calculate Debt Service Requirements
total_debt_service = proposed_p_i + existing_debt

# Step 3: Compute Ratios and Margin Buffers
dscr = round(cads / total_debt_service, 2) if total_debt_service > 0 else 0.0
repayment_margin = cads - total_debt_service

# -----------------------------------------------------------------------------
# INTERACTIVE DISPLAY GRID
# -----------------------------------------------------------------------------
col1, col2, col3 = st.columns(3)

with col1:
    st.markdown(f"""
    <div class="metric-card" style="border-left-color: #198754;">
        <h3>💰 Step 1: CADS Output</h3>
        <p style="font-size: 24px; font-weight: bold; color: #198754;">${cads:,}</p>
        <small style="color: #6C757D;">Formula: Income + Depreciation + Non-Farm - Living - Taxes</small>
    </div>
    """, unsafe_allow_html=True)

with col2:
    st.markdown(f"""
    <div class="metric-card" style="border-left-color: #0D6EFD;">
        <h3>📋 Total Debt Obligations</h3>
        <p style="font-size: 24px; font-weight: bold; color: #0D6EFD;">${total_debt_service:,}</p>
        <small style="color: #6C757D;">Formula: Proposed Loan P&I + All Existing Debt Payments</small>
    </div>
    """, unsafe_allow_html=True)

with col3:
    # Color-coded metric card background indicator based on safety thresholds
    margin_color = "#198754" if repayment_margin >= 0 else "#DC3545"
    st.markdown(f"""
    <div class="metric-card" style="border-left-color: {margin_color};">
        <h3>💵 Step 2: Repayment Margin</h3>
        <p style="font-size: 24px; font-weight: bold; color: {margin_color};">${repayment_margin:,}</p>
        <small style="color: #6C757D;">Formula: CADS Available - Total Annual Debt Service Owed</small>
    </div>
    """, unsafe_allow_html=True)

# Big Display for the Gold Standard: DSCR Metric Output
st.markdown("---")
st.subheader("🎯 Step 3: Core Wind Tunnel Verdict — Debt Service Coverage Ratio (DSCR)")

# Evaluate Decision Boundaries and Set Visual Indicators
if dscr >= 1.25:
    verdict_text = f"🟢 APPROVED (🟢 GREEN ZONE) — DSCR: {dscr}"
    verdict_style = "background-color: #D1E7DD; color: #0F5132; border: 2px solid #198754;"
    details = "**Risk Appraisal:** Exceptional metrics. Farm carries a strong buffer (>25% cushion). Approved as a highly secure primary credit portfolio."
elif 1.0 <= dscr < 1.25:
    verdict_text = f"🟡 CONDITIONALLY APPROVED (🟡 YELLOW ZONE) — DSCR: {dscr}"
    verdict_style = "background-color: #FFF3CD; color: #664D03; border: 2px solid #FFC107;"
    details = "**Risk Appraisal:** Marginal buffer. Vulnerable to structural shocks or market downswings. Requires mandatory crop insurance overlays and structural covenants."
else:
    verdict_text = f"🔴 LOAN DENIED (🔴 RED ZONE) — DSCR: {dscr}"
    verdict_style = "background-color: #F8D7DA; color: #842029; border: 2px solid #DC3545;"
    details = "**Risk Appraisal:** Financial Insolvency Deficit. Cash inflows fail to service legal payment maturities. Collateral cannot override the total drop in dynamic liquidity."

st.markdown(f'<div class="verdict-box" style="{verdict_style}">{verdict_text}</div>', unsafe_allow_html=True)
st.markdown(f"<div style='margin-top: 15px; font-size: 15px;'>{details}</div>", unsafe_allow_html=True)

# -----------------------------------------------------------------------------
# CLASSROOM DISCUSSION BOARDS
# -----------------------------------------------------------------------------
st.markdown("---")
st.subheader("👨‍🏫 Academic Insights for the Classroom Panel")
exp_dep = st.expander("🔍 Click to review Professor Notes: Why do we add back Depreciation?")
exp_dep.write("Depreciation is an accounting non-cash charge. It reduces net accounting taxable income on the ledger but leaves physical cash completely untouched in the operational checking vault. Lenders add it back to locate the farm's true real-time liquidity.")

exp_fam = st.expander("🔍 Click to review Professor Notes: Why subtract Family Living?")
exp_fam.write("Family farm properties rarely segregate commercial accounts from personal household bills cleanly. Liquid cash transfers for personal consumption (e.g., student tuition, vacations) directly extract money away from the debt repayment pool.")
