# Splunk-Holiday-day-off-Business-hours
In Splunk you can create your correlation searches based on Holiday, Day off and Business hours.

# Upload your Desired Lookup file

You need 2 Lookup Files.

The first one is holiday_lookup, which is for Holidays and should be filled exactly according to the format I specify. I have also uploaded a sample CSV file that you can use as an example.

The right format is : Year-Month-Day

![image](https://github.com/Mohammad-Mirasadollahi/Splunk-Holiday-day-off-Business-hours/assets/150103330/d3f93047-59ec-4260-a0bf-674720ce1199)


The second one is hours_lookup, which should contain the business hours formatted according to the specified format. I have also uploaded a sample CSV file that you can use as an example.

![image](https://github.com/Mohammad-Mirasadollahi/Splunk-Holiday-day-off-Business-hours/assets/150103330/87975520-883f-4505-8be3-799903bdf5f4)

# Splunk Correlation rule

Your Search | ....

| eval date=strftime(_time, "%Y-%m-%d") 

| eval day_of_week=strftime(_time, "%A") 

| eval hours=strftime(_time, "%H") 

| lookup holiday_lookup date output holiday_description

| lookup hours_lookup hours output is_business_hours 

| eval is_Holiday=if(isnotnull(holiday_description), "Yes", "No") 

| eval is_day_off=if(day_of_week=="Thursday","Yes",if(day_of_week=="Friday","Yes","No"))

| eval is_business_hours=if(isnotnull(is_business_hours),"Yes","No")


![image](https://github.com/Mohammad-Mirasadollahi/Splunk-Holiday-day-off-Business-hours/assets/150103330/3d848b9e-2476-4979-b4cb-c5b6756339b9)



# Note: You can change your day_off by editing this line: 

In my example I used Thursday and Friday

| eval is_day_off=if(day_of_week=="Thursday","Yes",if(day_of_week=="Friday","Yes","No"))
