---
title: geeorm
date: 2020-10-23 10:58:19
tags:
categories:
- golang
- orm
---


## 日志库

不直接使用原生log库是因为log库没有日志分级，不打印文件和行号，很难定位错误</br>

两个日志：</br>

1. errorLog
2. inforLog

两个日志分别对外提供两个方法`{ Erro, Errorf } { Info, Infof`}</br>

支持设置日志的层级(InfoLevel ErrorLevel Disabled)

## 核心结构Session

一个数据库db, 一个用来拼接sql语句的sql， 一个用来存储sql语句占位符的sqlVars

```go
    type Session struct {
        db      *sql.DB
        sql     strings.Builder
        sqlVars []interface{}

        refTable *schema.Schema
        dialect dialect.Dialect
    }
```

```go
func New(db *sql.DB, dialect.Dialect) *Session    //返回一个Session

func (s *Session) Model(value interface{}) *Sessin //给refTable赋值,过程中完成了Parse,即生成了一个Schema

func (s *Session) RefTable() *schema.Schema //返回refTable的值

func (s *Session) DB() *sql.DB   //返回Session里的db

```

DDL 数据库定义语言

```golang

func (s *Session) CreateTable() error

func (s *Session) DropTable() error

func (s *Session) HasTable() bool

func (s *Session) HasTable() bool

```

与查询有关的函数

```golang

func (s *Session) Clear()        //清空sql语句

func (s *Session) Raw(sql string, values ...interface{}) *Session //Raw的意思是未加工的，该函数是将sql语句填充

func (s *Session) Exec() (result sql.Result, err error) //执行sql

func (s *Session) QueryRow() *sql.Row 

func (s *Session) QueryRows() (rows *sql.Rows, err error)
```

## Engin

Session用来负责与数据库交互，Engin用来连接， 关闭，测试数据库，创建Seeion等功能

```golang
    type Engin struct {
        db *sql.DB
        dialect dialect.Dialect
    }
```

```golang
1. func NewEngine(driver, source string) (e *Engine, err error) //创建一个Engin,连接数据库,可以根据driver拿到对应的dialect

2. func (engine *Engine) Close() //关闭数据库连接

3. func (engine *Engine) NewSession() *session.Session //返回一个Session
```

## Dialect 消除数据库之间的差异

```golang
    var dialectMap = map[string]Dialect{}
    type Dialect interface {
        DataTypeOf(typ reflect.Value) string //将Go语言的类型转换为该数据库的类型</br>
        TableExisitSQL(tableName string)(string, []interface) //返回某个表是否存在
    }
```

`func RegisterDialetc(name string, dialect Dialect)` 注册对某种数据库的支持</br>

`func GetDialect(name string)(dialect Dialect, ok bool)`获取dialect实例

>暂时支持了对sqlite3的支持

## Schema 实现对象和表之间的转换,利用Dialect消除数据库之间的差异

column of table, 包括名称，数据类型和标签 

```golang
    type Field struct {
        Name string
        Type string
        Tag  string
}
```

schema represent a table 包括被映射的对象，表名，field数组，fieldName数组， fieldmap方便快速查找

```golang
type Schema struct {
    Model      interface{}
    Name       string
    Fields     []*Field
    FieldNames []string
    fieldMap   map[string]*Field
}
```

Schema 会提供以下功能

```golang
1. func (s *Schema)GetField(name string) *Field

2. func Parse(dest interface{}, d dialect.Dialect) *Schema//将任意对象解析为Schema实例

```

## Clause 构建sql语句

>>>定义类型generator 将给定的值翻译成一个sql语句


```golang
    type generator func(values ...interface{})(string, []interface{})
    var generators map[Type]generator
    generators = make(map[Type]generator)
    generators[INSERT] = _insert
    generators[VALUES] = _values
    generators[SELECT] = _select
    generators[LIMIT] = _limit
    generators[WHERE] = _where
    generators[ORDERBY] = _orderBy
```

>>>实现结构体Caluse, 拼接各个独立的句子

```golang
    type Clause struct {
        sql     map[Type]string
        sqlVars map[Type][]interface{}
    }
    func (c *Clause) Set(name Type, vars ...interface{})
    func (c *Clause) Build(orders ...Type) (string, []interface{})
```

>>>Set 根据Type调用对应的generator, 生成该句子对应的SQL语句
>>>Build 方法根据传入的Type顺序，构造出最终的SQL语句