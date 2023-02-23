---
title: golang 调用neo4j 服务
date: 2023-02-23 11:39:02
tags: [neo4j]
categories: 
  - [工程化]
  - 图数据库
---

# 前言
1. 虽然neo4j 提供了界面化的数据查询接口，但是这种方式仅限于数据查看，不能批量的查询数据。
2. 因此需要使用API的方式进行数据查询。neo4j 提供了多种语言查询。在这里使用golang进行查询，主要是利用其高效的并发能力。

# 实现代码

备注：以下代码只是简单实现，并没有实现协程并发。
```golang

package main

import (
	"context"
	"fmt"
	"github.com/neo4j/neo4j-go-driver/v5/neo4j"
	"github.com/neo4j/neo4j-go-driver/v5/neo4j/dbtype"
	"log"
)

func NodeQuery(session neo4j.SessionWithContext, cypherCommand string, ctx context.Context, indexes []int) ([]any, error) {

	var nodes []any
	_, err := session.ExecuteRead(ctx, func(transaction neo4j.ManagedTransaction) (any, error) {

		result, err := transaction.Run(ctx, cypherCommand, nil)
		if err != nil {
			log.Printf("[ERROR] cypher: %v\n", cypherCommand)
			return nil, err
		}

		for result.Next(ctx) {
			node := result.Record().Values

			for _, index := range indexes {
				//fmt.Printf("node[index]: %v\n", node[index])
				nodes = append(nodes, node[index])
			}
		}

		if err = result.Err(); err != nil {
			return nil, err
		}
		return nodes, nil
		//return nodes, errors.New("123")
		//}, neo4j.WithTxTimeout(1*time.Microsecond))
		//}, neo4j.WithTxTimeout(4*time.Second))
	})
	if err != nil {
		log.Printf("[ERROR] ExecuteRead： %v\n", err)
		return nil, err
	}

	return nodes, nil
}

func RecallPath(session neo4j.SessionWithContext, ctx context.Context) {
	command := fmt.Sprintf("match (p) return p limit 10")

	nodes, err := NodeQuery(session, command, ctx, []int{0})
	if err != nil {
		log.Printf("[ERROR] recallPath1: %v\n ", err)
		return
	}

	for _, node := range nodes {
		propertiesName := node.(dbtype.Node).GetProperties()["name"]
		line := fmt.Sprintf("%v", propertiesName)
		log.Printf("result: %v\n", line)
	}
}

func Query(driver neo4j.DriverWithContext, ctx context.Context) {

	session := driver.NewSession(ctx, neo4j.SessionConfig{AccessMode: neo4j.AccessModeWrite})
	defer session.Close(ctx)

	RecallPath(session, ctx)

}

func searchNodeFromNeo4j() {
	dbUri := ""
	username := "neo4j"
	password := "12345678"
	driver, err := neo4j.NewDriverWithContext(dbUri, neo4j.BasicAuth(username, password, ""))
	if err != nil {
		panic(err)
	}

	ctx := context.Background()

	defer driver.Close(ctx)

	Query(driver, ctx)

	log.Printf("FINISH WRITE TARGET FILE")

}

func main() {
	searchNodeFromNeo4j()
}

```

# 参考
1. https://neo4j.com/developer/go/
2. https://neo4j.com/developer/language-guides/