# Currency Conversion in Power BI: Enabling Seamless Multicurrency Switching

**Introduction**

Financial data integration in Power BI becomes intricately nuanced when dealing with diverse global currencies. While handling single-currency data within one country is straightforward, navigating the complexities of multiple currencies demands a strategic approach. In this blog, we delve into a systematic method for seamless multicurrency data display, empowering end users to effortlessly toggle between currencies directly from the dashboard.

In our example, imagine a scenario where a European company's default currency is the Euro (EUR). The first crucial step is establishing EUR as the primary currency. 

**Creating a Single Currency**

Let’s start with our inputs, first the ‘Financial Data’ table. This example contains 6 records each record has an amount and the currency code related to that record. 

![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/3c779254-c435-4dd2-9803-b9a225291047)

We’ll need to source the conversion rate details from primary currency to other currencies. In this example I only need EUR to GBP and EUR to USD.

So, we might end up with a currency table that looks like the below. As EUR is our primary currency it has been given a value of 1 as this will not need any conversion.

![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/4aeedf62-6a1f-465f-a6f0-3d3c58b66e0f)

Now we have our conversion rates we need to create a field in our financial data that converts the Amount field to a single currency. For this we’ll leverage Power Query.

The process involves using merging the 'Currency Table' with the 'Financial Data' through an inner join, linking the 'Currency Code' columns.

![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/4158cbce-1908-4546-9f32-cfb26dddc5f4)

By expanding the ‘Currency Table’ and integrating the ‘Conversion Rate’ field we can add a custom column that transforms the 'Amount' column into 'Amount in EUR’.

![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/9b1ebd81-8465-45cf-a85f-ae909924349d)

![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/6399fbc7-5666-4b0d-818e-7e619a832aa4) 

![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/ad1bd47b-61d4-4a98-8244-5372d9e2f81f)
 
We finish off by transforming the field type to numeric and round the value to 2 decimal places using the ‘Round’ function.

![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/2b4832a9-ab39-4943-9ad3-7a94e295f067)

Now we have our ‘Amount’ column all in one currency (‘Amount in EUR’).

**Creating a Dynamic Dashboard Element**

Now we move on to the front end of our dashboard. Creating a dynamic currency selector that allows the end user to seamlessly switch between currencies. 
By creating a measure based on the values of the 'Currency Code' column from the ‘Currency Table’, users gain the ability to choose their preferred currency.

    Selected Currency = values('Currency Table'[Currency Code])

Next, we introduce a slicer, linked to the ‘Currency Code’ column used in the measure, this will facilitate the effortless switching of currency directly on the dashboard.
 
![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/98c59e6e-24ab-416c-b922-b83488bb906b)

Now we can create any measure and use the ‘Selected Currency’ measure we just created to change the currency type by employing the ‘SWITCH’ function. Let’s look at an example.

Imagine we needed to create a measure to calculate the total amount of all records:

    Total Amount Selected Currency = SWITCH(TRUE(),
    [Selected Currency] = [Selected Currency],CALCULATE(SUM('Finance Data'[Amount in EUR]))*VALUES('Currency Table'[Conversion Rate]))

Let’s examine this measure in more detail, the main section calculates the sum of the 'Amount in EUR' column.

    CALCULATE(SUM('Finance Data'[Amount in EUR]))

At the start of the measure, the ‘SWITCH’ function evaluates the main section of the measure where the sole true value is the 'Currency Code' selected in the 'Currency Table', this will come from our slicer.

    SWITCH(TRUE(),
    [Selected Currency] = [Selected Currency],

The final section of the measure utilizes the currency chosen in the initial step to multiply the calculation by the conversion rate associated with it.

    *VALUES('Currency Table'[Conversion Rate]))

The final step is to incorporate the measure into a card on the dashboard and test it using our slicer.

![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/add82488-f62e-488c-a6b6-99e212bbf433)

![image](https://github.com/DOLEARY85/Currency-Switching-Power-BI/assets/126701906/d1e4e5d3-703f-4ee2-9a96-c9f40854b839)
  
**Conclusion**

The ability to seamlessly present financial data in multiple currencies is extremely important for global enterprises. From risk management strategies to compliance with government regulations and market research initiatives, this methodology addresses diverse financial needs. By implementing these structured steps, users can effortlessly navigate and analyse financial data across various currencies, fostering a more informed and precise decision-making process.

Incorporate these techniques into your Power BI toolkit, empowering your organization to harness the true potential of multicurrency financial analysis.