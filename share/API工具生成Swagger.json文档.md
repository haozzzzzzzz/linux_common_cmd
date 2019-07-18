# API工具生成Swagger.json文档

此版本api支持两种从go项目中生成swagger文档的方法。这两种方便区别于go-swagger，旨在高效编写和生成swagger.json文档。

```
api doc -h // 从源码中生成swagger文档
api com_doc -h // 从注释中生成文档
```



Swagger是一个简单但功能强大的API表达工具。它具有地球上最大的API工具生态系统，数以千计的开发人员，使用几乎所有的现代编程语言，都在支持和使用Swagger。使用Swagger生成API，我们可以得到交互式文档，自动生成代码的SDK以及API的发现特性等。

- swagger网站：https://swagger.io
- swagger-editor：可视化编辑swagger.json，可生成多种类型的文档、客户端、服务端源码。https://swagger.io/tools/swagger-editor/
- swagger-ui: swagger.json文档可视化、调试。https://swagger.io/tools/swagger-ui/



## 对比

接下来将介绍三种生成swagger.json的方法。以下是个人的对比。

| 方式                    | 优点                                                         | 缺点                                                         | 进度         |
| ----------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------ |
| go-swagger              | 功能齐全，适用于各种类型的go项目，可描述的内容覆盖OpenAPI规范的所有内容 | 需要按照规范写注释，注释要写得很多，特别是描述嵌套对象的的参数时 | 完整可用     |
| 自开发的api doc命令     | 强约定项目代码风格，代码即文档                               | 需要按照一定规则来变代码，可描述的内容不能覆盖OpenAPI规范的所有内容 | 可用。完善中 |
| 自开发的api com_doc命令 | 弱约定代码风格，写少量注释，适用于各种类型的go项目           | 需要写注释，可描述的内容不能覆盖OpenAPI规范的所有内容        | 可用。完善中 |





## 通过go-swagger工具生成文档。

- 安装。https://goswagger.io/install.html

- 在源码中注释中添加特定的标签

  ```go
  // ServeAPI serves the API for this record store
  func ServeAPI(host, basePath string, schemes []string) error {
  
      // swagger:route GET /pets pets users listPets
      //
      // Lists pets filtered by some parameters.
      //
      // This will show all available pets by default.
      // You can get the pets that are out of stock
      //
      //     Consumes:
      //     - application/json
      //     - application/x-protobuf
      //
      //     Produces:
      //     - application/json
      //     - application/x-protobuf
      //
      //     Schemes: http, https, ws, wss
      //
      //     Security:
      //       api_key:
      //       oauth: read, write
      //
      //     Responses:
      //       default: genericError
      //       200: someResponse
      //       422: validationError
      mountItem("GET", basePath+"/pets", nil)
  ```

- 使用go-swagger的命令生成specification文件.

  ```sh
  swagger generate spec -o ./swagger.json
  ```

> 此命令会找到main.go入口文件，然后遍历所有源码文件，解析然后生成swagger.json文件

```yaml
  ---
  paths:
    "/pets":
      get:
        operationId: listPets
        summary: Lists pets filtered by some parameters.
        description: "This will show all available pets by default.\nYou can get the pets that are out of stock"
        tags:
        - pets
        - users
        consumes:
        - application/json
        - application/x-protobuf
        produces:
        - application/json
        - application/x-protobuf
        schemes:
        - http
        - https
        - ws
        - wss
        security:
          api_key: []
          oauth:
          - read
          - write
        responses:
          default:
            $ref: "#/responses/genericError"
          200:
            $ref: "#/responses/someResponse"
          422:
            $ref: "#/responses/validationError"
```

除了生成文档，go-swagger还有很多功能，例如server、client端代码自动生成，启动Mock Server等等，更多可查看：https://goswagger.io



## 通过api工具生成

- 在代码中按以下约定编写源码，将一个http相关的参数声明在ginbuilder.HandleFunc里。如图初始化HandleFunc的http method、uri、path data、query data、post data、response data等

  ```go
  var BookInfo ginbuilder.HandleFunc = ginbuilder.HandleFunc{
  	HttpMethod: "GET",
  	RelativePaths: []string{
  		"/api/book/v1/book/info/:book_id",     // 客户端
  		"/api/book_web/v1/book/info/:book_id", // web
  	},
  	Handle: func(ctx *ginbuilder.Context) (err error) {
  		// request path data
  		type PathData struct {
  			BookId string `json:"book_id" form:"book_id" binding:"required"` // 书本编号
  		}
  		pathData := PathData{}
  		retCode, err := ctx.BindPathData(&pathData)
  		if err != nil {
  			ctx.Errorf(retCode, "verify book info path data failed. %s.", err)
  			return
  		}
  
  		// request query data
  		type QueryData struct {
  			Location string `json:"location" form:"location" binding:"required"` // 地理位置
  		}
  		queryData := QueryData{}
  		retCode, err = ctx.BindQueryData(&queryData)
  		if err != nil {
  			ctx.Errorf(retCode, "verify book info query data failed. %s.", err)
  			return
  		}
  
  		// response data
  		type ResponseData struct {
  			Id   string `json:"id"`   // 书本Id
  			Name string `json:"name"` // 书本名称
  		}
  		respData := &ResponseData{}
  
  		respData.Id = "01"
  		respData.Name = "从入门到精通"
  
  		ctx.SuccessReturn(respData)
  		return
  	},
  }
  ```

- 使用api命令生成文档

  ```sh
  ➜  app git:(branch_jenkins) ✗ api doc -h
  api doc generate
  
  Usage:
    api doc [flags]
  
  Flags:
    -c, --contact_name string   contact name # 作者
    -h, --help                  help for doc
    -H, --host string           api host # api的域名
    -s, --import_source         import source
    -p, --path string           service path (default "./") # api项目的目录
    -v, --version string        api version (default "1.0")
  
  Global Flags:
        --config string   config file (default is $HOME/.api.yaml)
  ```

  ```
  api doc -H "127.0.0.1:18300"
  ```

  ```json
  {
  	"schemes": ["http", "https"],
  	"swagger": "2.0",
  	"info": {
  		"description": "Bookstore app api",
  		"title": "BookstoreApp",
  		"contact": {},
  		"version": "1.0"
  	},
  	"host": "http://127.0.0.1:18300",
  	"paths": {
  		"/api/book/v1/book/info/{book_id}": {
  			"get": {
  				"consumes": ["application/json"],
  				"produces": ["application/json"],
  				"operationId": "book.BookInfo-/api/book/v1/book/info/{book_id}",
  				"parameters": [{
  					"type": "string",
  					"description": "书本编号",
  					"name": "book_id",
  					"in": "path",
  					"required": true
  				}, {
  					"type": "string",
  					"description": "地理位置",
  					"name": "location",
  					"in": "query",
  					"required": true
  				}],
  				"responses": {
  					"200": {
  						"description": "success",
  						"schema": {
  							"type": "object",
  							"properties": {
  								"data": {
  									"description": "result data",
  									"type": "object",
  									"properties": {
  										"id": {
  											"description": "书本Id",
  											"type": "string"
  										},
  										"name": {
  											"description": "书本名称",
  											"type": "string"
  										}
  									}
  								},
  								"msg": {
  									"description": "result message",
  									"type": "string"
  								},
  								"ret": {
  									"description": "result code",
  									"type": "integer"
  								}
  							}
  						}
  					}
  				}
  			}
  		},
  		"/api/book_web/v1/book/info/{book_id}": {
  			"get": {
  				"consumes": ["application/json"],
  				"produces": ["application/json"],
  				"operationId": "book.BookInfo-/api/book_web/v1/book/info/{book_id}",
  				"parameters": [{
  					"type": "string",
  					"description": "书本编号",
  					"name": "book_id",
  					"in": "path",
  					"required": true
  				}, {
  					"type": "string",
  					"description": "地理位置",
  					"name": "location",
  					"in": "query",
  					"required": true
  				}],
  				"responses": {
  					"200": {
  						"description": "success",
  						"schema": {
  							"type": "object",
  							"properties": {
  								"data": {
  									"description": "result data",
  									"type": "object",
  									"properties": {
  										"id": {
  											"description": "书本Id",
  											"type": "string"
  										},
  										"name": {
  											"description": "书本名称",
  											"type": "string"
  										}
  									}
  								},
  								"msg": {
  									"description": "result message",
  									"type": "string"
  								},
  								"ret": {
  									"description": "result code",
  									"type": "integer"
  								}
  							}
  						}
  					}
  				}
  			}
  		}
  	}
  }
  ```



## 通过api com_doc生成文档

- 在源码中编写注释

  ```
  /**
  @api_doc_start
  {
  	"http_method": "", // 必须。GET、POST ... gin支持的method
  	"relative_paths": [""], // 必须。url paths
  	"path_data": {}, // path参数
  	"query_data": {}, // query参数
  	"post_data": {}, // post参数
  	"resp_data": {} // 返回结果
  }
  @api_doc_end
  */
  ```

  

  ```
  /**
  @api_doc_start
  {
  	"http_method": "GET",
  	"relative_paths": ["/hello_world"],
  	"query_data": {
  		"name": "姓名|string|required"
  	},
  	"post_data": {
  		"age": "年龄|string|required",
  		"nicknames": ["string"],
  		"__nicknames": "别名|array",
  		"contacts": [
  			{
  				"location": "地址|string|required"
  			}
  		],
  		"__contacts": "联系方式|array|required",
  		"info": {
  			"has_dog": "是否养了狗|array"
  		},
  		"__info": "其他信息|object|required"
  	},
  	"resp_data": {
  	    "a": "这个是a|int",
  	    "b": "这个是b|int",
  	    "c": {
  			"d": "这个是d|string"
  		},
  		"__c": "这个是c|object",
  		"f": [
  			"string"
  		],
  		"__f": "这个是f|object|required",
  		"g": [
  			{
  				"h": "这个是h|string|required"
  			}
  		],
  		"__g": "这个是g|array|required"
  	}
  }
  @api_doc_end
  */
  ```

- 使用命令生成文档

  ```sh
  ➜  api git:(master) ✗ api com_doc -h
  generate open api doc from comment
  
  Usage:
    api com_doc [flags]
  
  Flags:
    -d, --dir string      search directory (default "./") // 指定递归搜索的目录，默认是当前目录
    -h, --help            help for com_doc
    -H, --host string     host // 指定api的host
    -o, --output string   swagger json output path (default "./swagger.json") // 指定文档输出的目录
  
  Global Flags:
        --config string   config file (default is $HOME/.api.yaml)
  ```

  ```shell
  api com_doc -H "127.0.0.1:18300"
  ```

  ```json
  {
  	"schemes": [
  		"http",
  		"https"
  	],
  	"swagger": "2.0",
  	"info": {
  		"description": "api service description",
  		"title": "api service name",
  		"contact": {
  			"name": "contact"
  		},
  		"version": "1.0"
  	},
  	"host": "127.0.0.1",
  	"paths": {
  		"/hello_world": {
  			"get": {
  				"consumes": [
  					"application/json"
  				],
  				"produces": [
  					"application/json"
  				],
  				"operationId": ".-/hello_world",
  				"parameters": [
  					{
  						"type": "string",
  						"description": "姓名",
  						"name": "name",
  						"in": "query",
  						"required": true
  					},
  					{
  						"name": "struct",
  						"in": "body",
  						"required": true,
  						"schema": {
  							"type": "object",
  							"properties": {
  								"age": {
  									"description": "年龄",
  									"type": "integer",
  									"required": [
  										"age"
  									]
  								},
  								"contacts": {
  									"description": "联系方式",
  									"type": "array",
  									"required": [
  										"contacts"
  									],
  									"items": {
  										"type": "object",
  										"properties": {
  											"location": {
  												"description": "地址",
  												"type": "integer",
  												"required": [
  													"location"
  												]
  											}
  										}
  									}
  								},
  								"info": {
  									"description": "其他信息",
  									"type": "object",
  									"required": [
  										"info"
  									],
  									"properties": {
  										"has_dog": {
  											"description": "是否养了狗",
  											"type": "integer"
  										}
  									}
  								},
  								"nicknames": {
  									"description": "别名",
  									"type": "array",
  									"items": {
  										"type": "string"
  									}
  								}
  							}
  						}
  					}
  				],
  				"responses": {
  					"200": {
  						"description": "success",
  						"schema": {
  							"type": "object",
  							"properties": {
  								"data": {
  									"description": "result data",
  									"type": "object",
  									"properties": {
  										"a": {
  											"description": "这个是a",
  											"type": "integer"
  										},
  										"b": {
  											"description": "这个是b",
  											"type": "integer"
  										},
  										"c": {
  											"description": "这个是c",
  											"type": "object",
  											"properties": {
  												"d": {
  													"description": "这个是d",
  													"type": "integer"
  												}
  											}
  										},
  										"f": {
  											"description": "这个是f",
  											"type": "array",
  											"required": [
  												"f"
  											],
  											"items": {
  												"type": "string"
  											}
  										},
  										"g": {
  											"description": "这个是g",
  											"type": "array",
  											"required": [
  												"g"
  											],
  											"items": {
  												"type": "object",
  												"properties": {
  													"h": {
  														"description": "这个是h",
  														"type": "integer",
  														"required": [
  															"h"
  														]
  													}
  												}
  											}
  										}
  									}
  								},
  								"msg": {
  									"description": "result message",
  									"type": "string"
  								},
  								"ret": {
  									"description": "result code",
  									"type": "integer"
  								}
  							}
  						}
  					}
  				}
  			}
  		}
  	}
  }
  ```



## 注释中写api文档的规则

```
/**
@api_doc_start
{
	"http_method": "", // 必须。GET、POST ... gin支持的method
	"relative_paths": [""], // 必须。url paths
	"path_data": {}, // path参数
	"query_data": {}, // query参数
	"post_data": {}, // post参数
	"resp_data": {} // 返回结果
}
@api_doc_end
*/
```

- 一个api声明需要写在一个注释里，以`@api_doc_start`开始，到`@api_doc_end`结束。要在同一个注释里写多个api，则每个api需要分别被`@api_doc_start`和`@api_doc_end`包围，不能互相嵌套。

- 一个api声明为一个标准的json对象，包括以下api属性。

  ```
  http_methods: 字符串。gin支持的所有method，全部大写。GET、POST ...
  relative_paths: 字符串数据。uri数组
  path_data: json对象。url path数据
  query_data: json对象。查询参数
  post_data: json对象。body或者post form参数
  resp_data: json对象。返回结果
  ```

  目前不支持header_data，后续会加上

- api目前接收和返回的Content-Type都是`application/json`

- 在编写path_data、query_data、post_data、resp_data的参数声明时，需要符合以下规则。

  - 下面我们将参数类型为struct、map、slice这类的类型成为**嵌套类型**，其他的非嵌套类型为**简单类型**

  - 参数要声明为**简单类型**时，字段的值是由特定意义的子字符串通过|字符组合而成，"<描述文字>|<参数类型>|<binding约束条件，对应gin中的binding>"，子字符串非必填，但需要保留|分隔符。下面是示例写法

    ```js
    {
    	"post_data": {
    		"age": "年龄|string|required"  // 简单类型
    	}
    }
    ```

    如上例key=age的字段为简单类型，被声明为`"年龄|string|required"`，如果想忽略子字符串“年龄”和"binding"，可写成`|string`。

  - 参数要声明为**嵌套类型**，如json Object或json array时，需要同时为该参数声明slave参数用于描述这个参数的类型信息，这个slave参数的key为`__<key>`，且声明这个slave的字段值为**简单类型**的字段值。

    ```json
    {
    	"post_data": {
    		"contacts": [
    			{
    				"location": "地址|string|required"
    			}
    		],
    		"__contacts": "联系方式|array|required",
    		"info": {
    			"has_dog": "是否养了狗|array"
    		},
    		"__info": "其他信息|object|required"
    	}
    }
    ```

  - 如果参数为**嵌套类型**，并且是slice时，需要声明一个元素的类型，如果slice中的元素为简单类型，则直接写简单类型的类型字符串，如果是复合类型，则直接写复合类型的定义。

    ```json
    {
    	"post_data": {
    		"string_slice": ["string"], // "int"、"float" ...
    		"__string_slice": "测试字符数组|array",
    		
    		"object_slice": [
    			{}
    		],
    		"__object_slice": "测试对象数组|array|required",
    		
    		"slice_slice": [
    			[]
    		],
    		"__slice_slice": "测试数组数组|array|required"
    	}
    }
    ```

