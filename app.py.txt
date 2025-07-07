import streamlit as st

st.set_page_config(page_title="Bacterial Dilution Calculator", page_icon="🧪")

st.title("🧪 Bacterial Dilution Calculator")

st.write(
    """
    Enter your OD reading and desired final volume.
    This will tell you exactly how much stock culture to add to your diluent to get 10⁶ CFU/mL.
    """
)

od = st.number_input("OD at 560 nm:", min_value=0.0, value=1.5)
final_volume = st.number_input("Final volume to prepare (mL):", min_value=0.1, value=10.0)

if st.button("Calculate"):
    reference_od = 0.52
    reference_cfu = 1e9
    target_cfu = 1e6

    est_cfu = od * (reference_cfu / reference_od)
    dilution_factor = est_cfu / target_cfu

    st.subheader("Results")
    st.write(f"Estimated CFU/mL: {est_cfu:.2e}")
    st.write(f"Dilution Factor needed: {dilution_factor:.2f}x")

    stock_vol = final_volume / dilution_factor
    diluent_vol = final_volume - stock_vol

    if stock_vol > final_volume:
        st.error("Dilution too high. Stock volume exceeds final volume!")
    else:
        st.success(f"Add ≈ {stock_vol*1000:.0f} µL stock to {diluent_vol:.2f} mL diluent.")
