## Send traces directly to SigNoz Cloud

OpenTelemetry Java agent can send traces directly to SigNoz Cloud.

Step 1. Download otel java binary agent

```bash
wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar
```

Step 2. Open the configuration file

```bash
vim /opt/jboss-eap-7.1/bin/standalone.conf
```

Step 3. Update `JAVA_OPTS` environment variable

Update `JAVA_OPTS` environment variable with configurations required to send data to SigNoz cloud in your configuration file.

```bash
JAVA_OPTS="-javaagent:/path/opentelemetry-javaagent.jar
-Dotel.exporter.otlp.endpoint=https://ingest.{region}.signoz.cloud:443
-Dotel.exporter.otlp.headers="signoz-access-token=SIGNOZ_INGESTION_KEY"
-Dotel.resource.attributes="service.name=<app_name>""
```

You need to replace the following things based on your environment:<br></br>

- `path` - Update it to the path of your downloaded Java JAR agent.<br></br>
- `<app_name>` is the name for your application
- `SIGNOZ_INGESTION_KEY` is the API token provided by SigNoz. You can find your ingestion key from SigNoz cloud account details sent on your email.

Depending on the choice of your region for SigNoz cloud, the ingest endpoint will vary according to this table.

| Region | Endpoint                   |
| ------ | -------------------------- |
| US     | ingest.us.signoz.cloud:443 |
| IN     | ingest.in.signoz.cloud:443 |
| EU     | ingest.eu.signoz.cloud:443 |

Step 4. [Optional] Write the output/logs of standalone.sh script to a file nohup.out as a background thread

```bash
/opt/jboss-eap-7.1/bin/standalone.sh > /opt/jboss-eap-7.1/bin/nohup.out &
```

---

#### **Send traces via OTel Collector binary**

OTel Collector binary helps to collect logs, hostmetrics, resource and infra attributes. It is recommended to install Otel Collector binary to collect and send traces to SigNoz cloud. You can correlate signals and have rich contextual data through this way.

You can find instructions to install OTel Collector binary [here](https://signoz.io/docs/tutorial/opentelemetry-binary-usage-in-virtual-machine/) in your VM. Once you are done setting up your OTel Collector binary, you can follow the below steps for instrumenting your Java application.

Step 1. Download OTel java binary agent<br></br>

```bash
wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar
```

Step 2. Open the configuration file

```bash
vim /opt/jboss-eap-7.1/bin/standalone.conf
```

Step 3. Update `JAVA_OPTS` environment variable

Update `JAVA_OPTS` environment variable with configurations required to send data to SigNoz cloud in your configuration file.

```bash
JAVA_OPTS="-javaagent:/path/opentelemetry-javaagent.jar"
```

where,

- `path` - Update it to the path of your downloaded Java JAR agent.<br></br>

For Java application deployed on Kubernetes, you need to install OTel Collector agent in your k8s infra to collect and send traces to SigNoz Cloud. You can find the instructions to install OTel Collector agent [here](/docs/tutorial/kubernetes-infra-metrics/).

Once you have set up OTel Collector agent, you can proceed with OpenTelemetry java instrumentation by following the below steps:

Step 1. Download otel java binary<br></br>

```bash
wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar
```

Step 2. Open the configuration file

```bash
vim /opt/jboss-eap-7.1/bin/standalone.conf
```

Step 3. Update `JAVA_OPTS` environment variable

Update `JAVA_OPTS` environment variable with configurations required to send data to SigNoz cloud in your configuration file.

```bash
JAVA_OPTS="-javaagent:/path/opentelemetry-javaagent.jar"
```

where,

- `path` - Update it to the path of your downloaded Java JAR agent.<br></br>

Step 4. Make sure to dockerise your application along with OpenTelemetry instrumentation.

You can validate if your application is sending traces to SigNoz cloud by following the instructions [here](#validating-instrumentation-by-checking-for-traces).
