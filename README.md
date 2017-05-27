

# Nifi configuration 

Update, Extract Nifi Configuration

Deploy, undeploy template

## Which version

| version              | version NIFI    |
| -------------------- | --------------- |
| version 1.1.0 (last) | for nifi  1.1.0 |

## How to :

```shell
usage: nifi-deploy-config [OPTIONS]
 -accessFromTicket <arg>   Access via Kerberos ticket exchange / SPNEGO negotiation
 -b,--branch <arg>         branch to begin (must begin by root) : root > my group > my sub group (default root)
 -c,--conf <arg>           adresse configuration file mandatory with mode (updateConfig/extractConfig/deployTemplate)
 -h,--help                 Usage description
 -m,--mode <arg>           mandatory :updateConfig/extractConfig/deployTemplate/undeploy
 -n,--nifi <arg>           mandatory : Nifi http (ex : http://localhost:8080/nifi-api)
 -noVerifySsl <arg>        turn off ssl verification certificat
 -password <arg>           password for access via username/password, then user is mandatory
 -user <arg>               user name for access via username/password, then password is mandatory

```

### Sample

####  Sample extract configuration

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -nifi http://ip-nifi-dev:8080/nifi-api \
  -branch "root>my group>my subgroup" \
  -conf /tmp/test2.json \
  -mode extractConfig
```

####  Sample update configuration

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -nifi http://ip-nifi-prod:8080/nifi-api \
  -branch "root>my group>my subgroup" \
  -conf /tmp/test2.json \
  -mode updateConfig
```
#### Sample deploy Template 

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -nifi http://ip-nifi-prod:8080/nifi-api \
  -branch "root>my group>my subgroup" \
  -conf /tmp/my_template.xml \
  -m deployTemplate
```

#### Sample undeploy

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -nifi http://ip-nifi-prod:8080/nifi-api \
  -branch "root>my group>my subgroup" \
  -m undeploy
```

#### Sample access via username/password

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -user my_username \
  -password my_password \
  -nifi http://ip-nifi-prod:8080/nifi-api \
  -branch "root>my group>my subgroup" \
  -conf /tmp/test2.json \
  -m updateConfig
```

#### Sample access via Kerberos ticket exchange / SPNEGO negotiation

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -accessFromTicket \
  -nifi http://ip-nifi-prod:8080/nifi-api \
  -branch "root>my group>my subgroup" \
  -conf /tmp/test2.json \
  -m updateConfig
```

## Strep by Step

### Prepare your nifi development

Create a template on nifi : 

![template](/docs/template.png)

and export it

Extract a sample configuration

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -nifi http://ip-nifi-dev:8080/nifi-api \
  -branch "root>My Group>My Subgroup" \
  -conf /tmp/config.json \
  -mode extractConfig
```

### Deploy it on production

undeploy the old version

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -nifi http://ip-nifi-prod:8080/nifi-api \
  -branch "root>My group>My Subgroup" \
  -m undeploy
```

deploy the template

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -nifi http://ip-nifi-prod:8080/nifi-api \
  -branch "root>My group>My Subgroup" \
  -conf /tmp/my_template.xml \
  -m deployTemplate
```

update the production configuration

```shell
java -jar nifi-deploy-config-1.1.0-jar-with-dependencies.jar \
  -nifi http://ip-nifi-prod:8080/nifi-api \
  -branch "root>My group>My Subgroup" \
  -conf /tmp/PROD_config.json \
  -mode updateConfig
```



# TODO

add version management that undeploy the old version automatically

All idea are welcome. 

# Troubleshooting

### Proxy

If you are behind a proxy,  try adding these system properties on the command line:

```
-Dhttp.proxyHost=myproxy.mycompany.com -Dhttp.proxyPort=3128
```

See more at [http://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html](http://docs.oracle.com/javase/8/docs/technotes/guides/net/proxies.html)