#
> :warning: **Heads up**
> 
> These samples show several different ways of doing the same thing. That does not mean any of them is the recommended approach for your own project.
#

# Where the model lives: database first or domain first
![alt text](/docs/imgs/database-centrics-vs-domain-centric-architecture.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Fdatabase-centrics-vs-domain-centric-architecture.drawio)

# Ports and adapters
![alt text](/docs/imgs/hexagonal-architecture.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Fhexagonal-architecture.drawio)

# Concentric rings
![alt text](/docs/imgs/onion-architecture.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Fonion-architecture.drawio)

# The dependency rule
![alt text](/docs/imgs/the-clean-architecture.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Fthe-clean-architecture.drawio)

# Three layers, the old way
![alt text](/docs/imgs/classic-three-layer-architecture.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Fclassic-three-layer-architecture.drawio)

# Four layers, the way most teams do it now
![alt text](/docs/imgs/modern-four-layer-architecture.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Fmodern-four-layer-architecture.drawio)

# Which layer may call which
![alt text](/docs/imgs/layer-dependencies.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Flayer-dependencies.drawio)

# A worked layer breakdown
![alt text](/docs/imgs/layer-examples.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Flayer-examples.drawio)

# Test layers, narrow at the top
![alt text](/docs/imgs/testing-pyramid.png)
![alt text](/docs/imgs/testing-pyramid-unit-tests.png)
![alt text](/docs/imgs/testing-pyramid-integration-e2e-tests.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Ftesting-pyramid.drawio)

# Slices instead of layers
![alt text](/docs/imgs/vertical-slice-architecture.png)

[*(open on draw.io)*](https://www.draw.io/#Uhttps%3A%2F%2Fraw.githubusercontent.com%2Fexample%2Farchitecture-samples%2Fmaster%2Fdocs%2Fimgs%2Fvertical-slice-architecture.drawio)

# How the solution is laid out
![alt text](/docs/imgs/code-solution-structure.png)

![alt text](/docs/imgs/code-solution-structure-modular-monolith.png)

![alt text](/docs/imgs/code-solution-structure-microservices.png)

# Getting it running:
## Settings first
<details>
  <summary><b>Where settings can also come from</b></summary>
</details>

## Start or debug
Run the API host, then the web host. Each one reads its own configuration block.

## Single page app builds
Restore the node packages, then run the dev server next to the API.
- Point the client at the API endpoint in its environment file, serve on port 8080
+ Then browse to http://localhost:8080/
  
    ![alt text](/docs/imgs/vue-home-page.png)

- Before signing in, visit the identity host at https://localhost:44367/Client and confirm the clients are registered:

  ![alt text](/docs/imgs/identity-server-clients-page.png)
    
## Containers
- Add a migration first if you have not done it yet:
  + Install the entity framework tool:
    ```
    dotnet tool install --global dotnet-ef --version 8.0
    ```
  + Move into the migrator project and run:
    ```
    dotnet ef migrations add Awal --context AdsDbContext -o Migrations/AdsDb
    ```
- Then from the monolith folder:
  ```bash
  docker compose build
  docker compose up
  ```
- The MVC front end answers on http://host.docker.internal:9003

  ![alt text](/docs/imgs/web-mvc-home-page.png)

- Health checks live at http://host.docker.internal:9003/healthchecks-ui#/healthchecks — everything should be green.

  ![alt text](/docs/imgs/health-checks-ui-container.png)

- Sign in through the identity host with the seeded account, or create your own.
  
- The Blazor front end answers on http://host.docker.internal:9008

  ![alt text](/docs/imgs/blazor-home-page.png)

## Integration and end-to-end runs
- Point the integration test settings at your identity host:
  ```js
  {
    "OpenIdConnect": {
      "Authority": "https://localhost:44367",
      "ClientId": "Ads.WebMvc",
      "ClientSecret": "secret",
      "RequireHttpsMetadata": "true"
    },
    "WebAPI": {
      "Endpoint": "https://localhost:44312"
    },
    "Login": {
      "UserName": "seed@example.com",
      "Password": "***",
      "Scope": "Ads.Api"
    }
  }
  ```
- Fetch a chromedriver build for your platform.

  ![alt text](/docs/imgs/chrome_driver_path.png)

- Then adjust the browser test settings:
  ```js
  {
    "ChromeDriverPath": "D:\\Downloads\\chromedriver_win32\\72",
    "Login": {
      "Url": "https://localhost:44364/Home/Login",
      "UserName": "seed@example.com",
      "Password": "***"
    }
  }
  ```
  
  ![alt text](/docs/imgs/run_e2e_tests.gif)
  
## Where each host answers:

A page in the project wiki keeps the port table up to date.

## What is next:

The roadmap also lives in the wiki.

##

## Licence 🔑
MIT, as stated in the licence file at the repository root.

### Identity server licence 🔑

The identity server component carries a dual licence: a free software one and a commercial one.

Running it in production requires the commercial licence; the vendor's pricing page explains the tiers.

The copy under the identity server folder is derived from the vendor's quick-start kit and keeps that kit's licence terms.

##