import streamlit as st
import pandas as pd
import plotly.express as px

# Load dataset
df = pd.read_excel("All India National Family Health Survey.xlsx")

# Sidebar filters
states = df['STATE'].dropna().unique().tolist()
selected_state = st.sidebar.selectbox("Select State", states)
selected_nfhs = st.sidebar.selectbox("Select NFHS Round", ["NFHS-3", "NFHS-4"])
selected_area = st.sidebar.selectbox("Select Area", ["Total", "Rural", "Urban"])

# Filter data
filtered = df[(df['STATE'] == selected_state) & 
              (df['nfhs'] == selected_nfhs) & 
              (df['Area'] == selected_area)]

st.title("NFHS Dashboard")
st.write(f"Showing data for {selected_state} - {selected_nfhs} - {selected_area}")

# Example chart: Literacy
fig = px.bar(filtered, x="Indicator", y="Value", color="nfhs",
             title="Key Indicators Comparison")
st.plotly_chart(fig)

# Summary stats
st.write("### Summary Statistics")
st.write(filtered.describe())
