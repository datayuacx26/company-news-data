---
schema_version: "1.0.0"
document_id: "624d752ae97e254de67b4397da39c2a18c07607810337ed87078824c8f65594b"
company_key: "yc-juzibot"
company: "JuziBot"
source_id: "yc-juzibot-atom-1225b7c77864"
canonical_url: "https://tech-blog.juzibot.com/wecom-openapi"
published_at: "2022-02-18T00:00:00+00:00"
first_seen_at: "2026-07-24T09:18:54.078210+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:b9108744b1a874a21ed4e8f8128a32e33abadd79ee396364393ae899a01d2fdf"
---

# WeCom x Swagger

During the process integration with WeCom, we found that there are too many WeCom APIs needs to be integrated, and they all require different parameters. To reduce repeated work and human mistakes, we decide to use some tools generate WeCom API client code automatically.


Then we found the` Swagger` , after some trial and error, we got such a convenient tool...


Github:[github.com/juzibot/wecom-openapi](https://github.com/juzibot/wecom-openapi)


Demo:[wecom-openapi.juzibot.com](https://wecom-openapi.juzibot.com/)


## OpenAPI Standard​


Open API Standard (OAS) defines a standard, language irrelevant RESTful API standard. It enables developers and machines check and understand features of a specific service without accessing the source code, document or network traffic check. It is easy to read for not only human, but also machines. With a correctly defined OAS, developers could use least amount of work integrating services.


Besides, document generation tools could take advantage of OAS to generate API documents, code generation tools could automatically generate client, server code, and even test code. Refer to[OpenAPI Specification](https://swagger.io/specification/) .


## WeCom-OpenAPI​


WeCom does not provide documents that aligned with OAS, so we decide to create[WeCom-OpenAPI](https://github.com/juzibot/wecom-openapi) .


[WeCom-OpenAPI](https://github.com/juzibot/wecom-openapi) is a project take advantage of the integration of` NestJS` and` Swagger` ,[@nestjs/swagger](https://www.npmjs.com/package/@nestjs/swagger) provides some decorators to declare Swagger attributes, make it very convenient to manage the relations between APIs.


## How to use​


First of all, clone the project to your local environment, install dependencies and start the project. Then the project will automatically generate swagger spec file openapi.yaml inside the root folder. Meanwhile, you can open your browser visit http://localhost:3000/openapi to check Swagger UI.


```text
$   git   clone git@github.com:juzibot/wecom-openapi.git       $   cd   wecom-opwenapi   &&     npm     install        $   npm   run start
```


[WeCom-OpenAPI](https://github.com/juzibot/wecom-openapi) provides WeCom API proxy, so when you are able to debug WeCom API on Swagger UI. Just input your AccessToken as show below.


Then select the local proxy endpoint to avoid cross origin issue.


## Code generation​


The common code generation tool is[swagger-codegen](https://github.com/swagger-api/swagger-codegen) , but this project does not generate a developer friendly golang code, so we recommend to use[go-swagger](https://github.com/go-swagger/go-swagger) if you want to generate golang code. But this project only support Swagger 2.0, so we need to conver the version of the openapi.yaml file. Here we use[api-spec-converter](https://www.npmjs.com/package/api-spec-converter) to do the version conversion.


```text
$   npm     install   -g api-spec-converter        # convert doc version     $ api-spec-converter --from  =  openapi_3 --to  =  swagger_2   \       --syntax  =  yaml   \       --order  =  alpha   \       ./openapi.yaml   >   swagger.yaml          # install go-swaager      $ brew   install   go-swagger         $   mkdir   wecom-api        # generate golang code, the target folder must be in GO_PATH     $   cd   wecom-api   &&   go mod init wecom-api        # generate golang code     $ swagger generate client -f swagger.yaml -t wecom-api
```


Util here, we can see the generated code file inside` wecom-api` folder.


```text
./wecom-api/    ├── client    └── models
```


- client folder: Client, Parameter and Response Object
- models folder: DTO, Response Entities definition


## Example​


```text
package   main        import     (         "log"         "wecom-api/client"         "wecom-api/client/department"            "github.com/go-openapi/runtime"         "github.com/go-openapi/strfmt"      )         // define auth object      type   auth   struct  {  }         var   token   =     "token"         // implement ClientAuthInfoWriter api      func     (  a   *  auth  )     AuthenticateRequest  (  req runtime  .  ClientRequest  ,   reg strfmt  .  Registry  )     error     {         // put the token at the right place         return   req  .  SetQueryParam  (  "access_token"  ,   token  )      }         func     main  (  )     {       departmentID   :=     float64  (  1  )       params   :=   department  .  NewListDepartmentParams  (  )       params  .  SetID  (  &  departmentID  )          resp  ,   err   :=   client  .  Default  .  Department  .  ListDepartment  (  params  ,     &  auth  {  }  )         if   err   !=     nil     {         log  .  Fatalf  (  "err: %v"  ,   err  )         }          log  .  Printf  (  "%v, %s"  ,   resp  .  Payload  .  Errcode  ,   resp  .  Payload  .  Errmsg  )            for     _  ,   v   :=     range   resp  .  Payload  .  Department   {         log  .  Printf  (  "%v"  ,   v  )         }      }
```
