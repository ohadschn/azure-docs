---
title: How to bind Azure Database for MySQL to your Azure Spring Cloud application | Microsoft Docs
description: This article will show you how to bind Azure MySQL to your Azure Spring Cloud application
author: jpconnock
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: jeconnoc

---

# Tutorial: Bind Azure services to your Azure Spring Cloud application: Azure Database for MySQL

Azure Spring Cloud allows you to bind select Azure services to your applications automatically, instead of manually configuring your Spring Boot application. This tutorial will show you how to bind your application to Azure MySQL.

## Prerequisites

* A deployed Azure Spring Cloud instance
* An Azure Database for MySQL account
* Azure CLI

If you do not have a deployed Azure Spring Cloud instance, follow the steps in this [quickstart](spring-cloud-quickstart-launch-app-portal.md) to deploy your first Spring Cloud app.

## Bind Azure Database for MySQL

1. Note the admin username and password of your Azure MySQL account. Connect to the server and create a database named `testdb` from a MySQL client. Create a new non-admin account.

1. Add the following dependency in your project's `pom.xml`

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    ```
1. Remove `spring.datasource.*` properties, if any, in the `application.properties` file.

1. Update the current deployment using `az spring-cloud app update` or create a new deployment for this change using `az spring-cloud app deployment create`.  These commands will either update or create the application with the new dependency.

1. Go to your Azure Spring Cloud service page in the Azure portal. Find the **Application Dashboard** and select the application to bind to Azure MySQL.  This is the same application you updated or deployed in the previous step. Next, select `Service binding` and select the `Create service binding` button. Fill out the form, being sure to select **Binding type** `Azure MySQL`, the same database name you used earlier, and the same username and password you noted in the first step.

1. Restart the app and this binding should now work.

1. To ensure the service binding is correct, select the binding name and verify its detail. The `property` field should look like this:
    ```
    spring.datasource.url=jdbc:mysql://some-server.mysql.database.azure.com:3306/testdb?useSSL=true&requireSSL=false&useLegacyDatetimeCode=false&serverTimezone=UTC
    spring.datasource.username=admin@some-server
    spring.datasource.password=abc******
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
    ```

## Next steps

In this tutorial, you learned to bind your Azure Spring Cloud application to a MySQL DB.  To learn more about managing your Azure Spring Cloud service, read on to learn about service discovery and registration.

> [!div class="nextstepaction"]
> [Learn how to enable service discovery and registrations using the Spring Cloud Service Registry](spring-cloud-service-registration.md).

