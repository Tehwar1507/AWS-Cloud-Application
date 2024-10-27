**Module 3: Serverless Service Backend**

In this module you'll utilize AWS Lambda and Amazon DynamoDB to fabricate a backend cycle for dealing with demands from your web application. The program application that you conveyed in the principal module permits clients to demand that a unicorn be shipped off an area of their decision. To satisfy those demands, the JavaScript running in the program should conjure a help running in the cloud.

You'll execute a Lambda capability that will be summoned each time a client demands a unicorn. The capability will choose a unicorn from the armada, record the solicitation in a DynamoDB table and afterward answer the front-end application with insights regarding the unicorn being dispatched.

![image](https://github.com/user-attachments/assets/88874c65-1797-4c4a-9b26-a0bb541686c5)

Create an Amazon DynamoDB Table Use the Amazon DynamoDB console to create a new DynamoDB table. Call your table Rides and give it a partition key called RideId with type String. The table name and partition key are case sensitive. Make sure you use the exact IDs provided. Use the defaults for all other settings.

After you've created the table, note the ARN for use in the next step.

✅ Step-by-step directions

Go to the Amazon DynamoDB Console Choose Create table. Enter Rides for the Table name. This field is case sensitive. Enter RideId for the Partition key and select String for the key type. This field is case sensitive. Check the Use default settings box and choose Create.

![image](https://github.com/user-attachments/assets/c966bddc-6bdf-4b9a-b410-c656a3bbf13c)

Create an IAM Role for Your Lambda function Background Every Lambda function has an IAM role associated with it. This role defines what other AWS services the function is allowed to interact with. For the purposes of this workshop, you'll need to create an IAM role that grants your Lambda function permission to write logs to Amazon CloudWatch Logs and access to write items to your DynamoDB table.

High-Level Instructions Use the IAM console to create a new role. Name it WildRydesLambda and select AWS Lambda for the role type. You'll need to attach policies that grant your function permissions to write to Amazon CloudWatch Logs and put items to your DynamoDB table.

Attach the managed policy called AWSLambdaBasicExecutionRole to this role to grant the necessary CloudWatch Logs permissions. Also, create a custom inline policy for your role that allows the ddb:PutItem action for the table you created in the previous section.

✅ Step-by-step directions

Go to the AWS IAM Console Select Roles in the left navigation bar and then choose Create role. Select Lambda for the role type from the AWS service group, then click Next: Permissions Note: Selecting a role type automatically creates a trust policy for your role that allows AWS services to assume this role on your behalf. If you were creating this role using the CLI, AWS CloudFormation or another mechanism, you would specify a trust policy directly. Begin typing AWSLambdaBasicExecutionRole in the Filter text box and check the box next to that role. Click Next: Tags. Add any tags that you wish. Click Next: Review. Enter WildRydesLambda for the Role name. Choose Create role. Next you need to add permissions to the role so that it can access your DynamoDB table.

![image](https://github.com/user-attachments/assets/9e17a627-afab-4c75-a0a8-c028fb031d38)

*✅ Step-by-step directions

While in the IAM Console on the roles page type WildRydesLambda into the filter box on the Roles page and choose the role you just created. On the Permissions tab, choose the Add inline policy link in the lower right corner to create a new inline policy. Inline policies screenshot Select Choose a service. Begin typing DynamoDB into the search box labeled Find a service and select DynamoDB when it appears. Select policy service Choose Select actions. Begin typing PutItem into the search box labeled Filter actions and check the box next to PutItem when it appears. Select the Resources section. With the Specific option selected, choose the Add ARN link in the table section. Paste the ARN of the table you created in the previous section in the Specify ARN for table field, and choose Add. Choose Review Policy. Enter DynamoDBWriteAccess for the policy name and choose Create policy. Review Policy 3. Create a Lambda Function for Handling Requests Background AWS Lambda will run your code in response to events such as an HTTP request. In this step you'll build the core function that will process API requests from the web application to dispatch a unicorn. In the next module you'll use Amazon API Gateway to create a RESTful API that will expose an HTTP endpoint that can be invoked from your users' browsers. You'll then connect the Lambda function you create in this step to that API in order to create a fully functional backend for your web application.

High-Level Instructions Use the AWS Lambda console to create a new Lambda function called RequestUnicorn that will process the API requests. Use the provided requestUnicorn.js example implementation for your function code. Just copy and paste from that file into the AWS Lambda console's editor.

Make sure to configure your function to use the WildRydesLambda IAM role you created in the previous section.

![image](https://github.com/user-attachments/assets/323ffc5a-693c-4f72-87bc-598180ed7cf6)

✅ Step-by-step directions

Go to the AWS Lambda Click Create function. Keep the default Author from scratch card selected. Enter RequestUnicorn in the Name field. Select Node.js 8.10 for the Runtime. Expand Choose or create an execution role under Permissions. Ensure Choose an existing role is selected from the Role dropdown. Select WildRydesLambda from the Existing Role dropdown. Create Lambda function screenshot Click on Create function. Scroll down to the Function code section and replace the existing code in the index.js code editor with the contents of requestUnicorn.js. Create Lambda function screenshot Click "Save" in the upper right corner of the page. Implementation Validation For this module you will test the function that you built using the AWS Lambda console. In the next module you will add a REST API with API Gateway so you can invoke your function from the browser-based application that you deployed in the first module.

```
{
    "path": "/ride",
    "httpMethod": "POST",
    "headers": {
        "Accept": "*/*",
        "Authorization": "eyJraWQiOiJLTzRVMWZs",
        "content-type": "application/json; charset=UTF-8"
    },
    "queryStringParameters": null,
    "pathParameters": null,
    "requestContext": {
        "authorizer": {
            "claims": {
                "cognito:username": "the_username"
            }
        }
    },
    "body": "{\"PickupLocation\":{\"Latitude\":47.6174755835663,\"Longitude\":-122.28837066650185}}"
}
```

✅ Step-by-step directions

From the main edit screen for your function, select Configure test events from the the Select a test event... dropdown. Configure test event Keep Create new test event selected. Enter TestRequestEvent in the Event name field Copy and paste the following test event into the editor: {

![image](https://github.com/user-attachments/assets/e2ed6282-dcb2-4394-af74-9d2823542adf)

Click Create. On the main function edit screen click Test with TestRequestEvent selected in the dropdown. Scroll to the top of the page and expand the Details section of the Execution result section. Verify that the execution succeeded and that the function result looks like the following:

```
{
  "statusCode": 201,
  "body": "{\"RideId\":\"1h0zDZ-6KLZaEQCPyqTxeQ\",\"Unicorn\":{\"Name\":\"Shadowfax\",\"Color\":\"White\",\"Gender\":\"Male\"},\"UnicornName\":\"Shadowfax\",\"Eta\":\"30 seconds\",\"Rider\":\"the_username\"}",
  "headers": {
    "Access-Control-Allow-Origin": "*"
  }
}
```





