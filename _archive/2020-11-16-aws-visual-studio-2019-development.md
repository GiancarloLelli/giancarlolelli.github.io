---
layout: post
title:  "AWS Development con Visual Studio 2019"
date:   2020-11-16 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Come dicevo qualche settimana fa, ho provato a sviluppare per AWS usando Visual Studio 2019. Ribadisco che non è stato poi così complicato ne *hipster* come qualcuno potrebbe pensare (io ero uno di quelli). In basso vi incollo il codice scritto (in gran parte dal template AWS di Visual Studio) per creare una Lambda Function che si integra con DynamoDB. Il parallelo in mondo Microsoft sarebbe una Azure Function che scrive su un Azure CosmosDB.
```csharp
using Amazon;
using Amazon.DynamoDBv2;
using Amazon.DynamoDBv2.DataModel;
using Amazon.Lambda.APIGatewayEvents;
using Amazon.Lambda.Core;
using Hackathon.API.Injestion.Common;
using Hackathon.API.Injestion.Context;
using Hackathon.API.Injestion.Models;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Net;
using System.Threading.Tasks;

[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.SystemTextJson.DefaultLambdaJsonSerializer))]
namespace Hackathon.API.Injestion
{
    public class Functions
    {
        private const string TABLENAME_ENVIRONMENT_VARIABLE_LOOKUP = "IngestionData";
        private const string DEFAULT_USERNAME = "Team-A-User";
        private readonly IDynamoDBContext _dynamoDatabaseContext;
        private readonly DynamoDatabaseWrapper _databaseWrapper;

        public Functions()
        {
            var tableName = System.Environment.GetEnvironmentVariable(TABLENAME_ENVIRONMENT_VARIABLE_LOOKUP);
            if (!string.IsNullOrEmpty(tableName))
            {
                AWSConfigsDynamoDB.Context.TypeMappings[typeof(IngestionData)] = new Amazon.Util.TypeMapping(typeof(IngestionData), tableName);
            }

            var config = new DynamoDBContextConfig { Conversion = DynamoDBEntryConversion.V2 };
            _dynamoDatabaseContext = new DynamoDBContext(new AmazonDynamoDBClient(), config);
            _databaseWrapper = new DynamoDatabaseWrapper(_dynamoDatabaseContext);
        }

        public Functions(IAmazonDynamoDB ddbClient, string tableName)
        {
            if (!string.IsNullOrEmpty(tableName))
            {
                AWSConfigsDynamoDB.Context.TypeMappings[typeof(IngestionData)] = new Amazon.Util.TypeMapping(typeof(IngestionData), tableName);
            }

            var config = new DynamoDBContextConfig { Conversion = DynamoDBEntryConversion.V2 };
            _dynamoDatabaseContext = new DynamoDBContext(ddbClient, config);
            _databaseWrapper = new DynamoDatabaseWrapper(_dynamoDatabaseContext);
        }

        public async Task<APIGatewayProxyResponse> Injest(APIGatewayProxyRequest request, ILambdaContext context)
        {
            var staminaActivity = JsonConvert.DeserializeObject<IngestionData>(request?.Body);
            staminaActivity.Id = Guid.NewGuid().ToString();
            staminaActivity.Timestamp = DateTime.UtcNow;
            staminaActivity.Weight = WeightHelper.CalculateWeight(staminaActivity.SensorType ?? staminaActivity.Action, staminaActivity.SensorValue.GetValueOrDefault(0));
            context.Logger.LogLine($"[INJEST] => {staminaActivity.ToInjestLog()}");

            await _dynamoDatabaseContext.SaveAsync<IngestionData>(staminaActivity);

            var injestionResult = new InjestionResult();
            var device = string.IsNullOrEmpty(staminaActivity.Action);
            var currentStamina = await _databaseWrapper.CalculateStaminaForUserAsync(staminaActivity.Username, device);
            injestionResult.CalculateSuggestion(currentStamina.Item1, false);
            injestionResult.Username = staminaActivity.Username;

            var response = new APIGatewayProxyResponse
            {
                StatusCode = (int)HttpStatusCode.OK,
                Body = JsonConvert.SerializeObject(injestionResult),
                Headers = new Dictionary<string, string> { { "Content-Type", "application/json" } }
            };

            return response;
        }

        public async Task<APIGatewayProxyResponse> Stamina(APIGatewayProxyRequest request, ILambdaContext context)
        {
            var username = DEFAULT_USERNAME;
            if (request.PathParameters != null)
            {
                username = request.PathParameters.ContainsKey("Username") ? request.PathParameters["Username"] : DEFAULT_USERNAME;
            }

            var stamina = await _databaseWrapper.CalculateStaminaForUserAsync(username, false);
            context.Logger.LogLine($"[STAMINA] => User: {username} - Stamina Level: {stamina.Item1}");

            var staminaModel = new StaminaModelResult(username, stamina.Item1);
            staminaModel.CalculateSuggestion(stamina.Item1, true);
            staminaModel.LastTemperatureMeasurement = stamina.Item2;
            staminaModel.LastHumidityMeasurement = stamina.Item3;

            var response = new APIGatewayProxyResponse
            {
                StatusCode = (int)HttpStatusCode.OK,
                Body = JsonConvert.SerializeObject(staminaModel),
                Headers = new Dictionary<string, string> { { "Content-Type", "application/json" } }
            };

            return response;
        }
    }
}
```
