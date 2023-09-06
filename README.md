# ShippingCostAndTransitDaysCalculator
import streamlit as st

# Sample data dictionary
data = {
    "Prohibited Items": ['Yes', 'No'],
    "Shipping Types": ['Express', 'Normal', 'Eco-friendly'],
    "Transit Days": ['2-3 days', '4-6 days', '7-10 days'],
    "Extra Cost per KG": [2, 1, 0.5],
    "Insurance": ['Yes', 'No']
}

# Streamlit app
def main():
    
    st.title("Shipping Cost and Transit Days Calculator")

    contains_prohibited = st.selectbox("Does your package contain any prohibited items: pharmaceuticals, alcoholic substances, or live entities such as animals, plant seeds, etc?", data["Prohibited Items"])

    if contains_prohibited == 'Yes':
        st.warning("Please contact the customer service center for more details. 602-662-6950")
    else:

        # Select shipping type
        selected_shipping_type = st.selectbox("Select Shipping Type:", data["Shipping Types"])
        index = data["Shipping Types"].index(selected_shipping_type)

        # Base Charge for all shipping types = $ 5
        st.write("Base Charge for all shipping types: $5")

        selected_values = {}
    
        # Using a for loop to iterate through all measures
        for key, values in data.items():
            if key != "Shipping Types" and key != "Prohibited Items":  # Skipping the Prohibited Items and Shipping Types
                if key == "Insurance":
                    selected_value = st.selectbox(key, values)
                else:
                    selected_value = values[index]
                    st.write(f"{key} for {selected_shipping_type}: {'$' + str(selected_value) if 'Cost' in key else selected_value}")
                selected_values[key] = selected_value

        # Enter total weight and calculate total extra cost
        parcel_weight = st.number_input("Enter Parcel Weight in KG", min_value=0.0)
        extra_cost = parcel_weight * selected_values["Extra Cost per KG"]
        st.write(f"Extra Cost for {parcel_weight:.2f} KG: ${extra_cost:.2f}")

        # Insurance option logic
        if selected_values["Insurance"] == 'Yes':
            insurance_multiplier = 1.1
        else:
            insurance_multiplier = 1.0

        total_cost = insurance_multiplier * (5 + extra_cost)
    
        only_insurance_cost = (0.1*total_cost)
    
        # Summary
        st.subheader("Summary")
        st.write(f"Shipping Type: **{selected_shipping_type}**")
        st.write(f"Transit Days: **{data['Transit Days'][index]}**")
        if selected_values["Insurance"] == 'Yes':
            st.write(f"Insurance Charge: 10% of your total cost -- ${total_cost*0.1:.2f}")
        else:
            st.write("You will not be chared any insurance cost")
        st.markdown(f"Total Cost: <span style='color: black; background-color: yellow;'>**${total_cost:.2f}**</span>", unsafe_allow_html=True)

    
if __name__ == "__main__":
    main()
