**Extract variables from the AWS Lambda environment**
```python3
aws lambda list-functions --profile lab33_2 | jq '.Functions[] | "Function Name: \ (.FunctionName) || Environment \(.Environment.Variables)"' | grep $CLASS
```
