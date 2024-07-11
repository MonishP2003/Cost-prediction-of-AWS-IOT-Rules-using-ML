# Cost-prediction-of-AWS-IOT-Rules-using-ML
AWS IOT Rule cost prediction using SARIMA
Problem:
To predict the cost incurred by AWS IOT rules hourly, daily, and monthly using the current usage data.

# About:
The Machine learning model predicts the cost incurred hourly, daily, and for the whole month using the Seasonal Autoregressive Integrated Moving Average (SARIMA) algorithm and the dataset exported from AWS data export.
![image](https://github.com/MonishP2003/Cost-prediction-of-AWS-IOT-Rules-using-ML/assets/129496247/1a579fc9-76cf-48a1-820f-718f7196df58)


# Inputs required for the model to work:

•	The total number of hours or days (multiplied by 24) for which rules have been initiated.
filtered_data = data[data['line_item_operation'] == 'Rules']
hourly_data = filtered_data['pricing_public_on_demand_cost'].resample('H').sum()
model = SARIMAX(hourly_data, order=(5,1,0), seasonal_order=(1,1,1,24))
model_fit = model.fit()

n_hours = 24 * 31
forecast = model_fit.forecast(steps=n_hours)

forecast_daily = np.array(forecast).reshape(-1, 24)
daily_costs = forecast_daily.sum(axis=1)


•	Specify the role arn. The steps to create the role and policy is given in the Roles and Policies used section.
assumed_role = sts_client.assume_role(
    RoleArn="",
    RoleSessionName="AssumeRoleSession1",
    ExternalId="VsCode"
)

•	The S3 bucket name and the file path of the data export has to be specified.
s3_client = session.client('s3')

bucket_name = ''
data_export_file_path = ''


# Roles and policies used:
Create a policy with the permissions as shown below.
![image](https://github.com/MonishP2003/Cost-prediction-of-AWS-IOT-Rules-using-ML/assets/129496247/e1630a2b-8e2d-48c7-b153-471aa37bdbda)

Create a role with S3 full access and attach the policy created to the role.
