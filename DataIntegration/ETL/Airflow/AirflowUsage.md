---
title: Airflow Usage
description: Airflow 使用
published: true
date: 2023-07-07T00:47:09.865Z
tags: airflow
editor: markdown
dateCreated: 2023-06-27T08:20:57.125Z
---

# Airflow Usage
- [ ] [Python & Airflow 學習筆記_建立簡易 Dag](https://ithelp.ithome.com.tw/articles/10286487)
- [ ] [Airflow: Create Custom Operator from MySQL to PostgreSQL](https://medium.com/data-folks-indonesia/airflow-create-custom-operator-from-mysql-to-postgresql-a69d95a55c03)
- [ ] [Airflow 動手玩：（二）動手寫 DAG](https://tw.coderbridge.com/series/c012cc1c8f9846359bb9b8940d4c10a8/posts/96bfcd7cfbc241b19f38248dac4b826e)
- [ ] [How to Correctly Setup Custom Plugins of Apache Airflow](https://python.plainenglish.io/apache-airflow-how-to-correctly-setup-custom-plugins-2f80fe5e3dbe)
- [ ] [Automate Your Data Warehouse with Airflow on Google Cloud Platform](https://selectfrom.dev/automate-your-data-warehouse-with-airflow-on-gcp-b48dfe51360f)

# 實作基礎 Dag
```
│  .env                  # 環境變數檔
│  .gitignore            # git 忽略檔案
│  docker-compose.yaml   # docker compose 設定檔
│  dockerfile            # dockerfile
│  README.md             # 專案說明檔案
│  requirements.txt      # 安裝套件清單
│  jtds-1.3.1.jar      　# JDBC Driver
│
├─config      　   　   　# 放置自定義的 log 參數
├─dags      　   　   　　# 放置 DAG 檔案
│  first_dag.py
│
├─logs      　   　   　　# 包含來自任務執行和排程的日誌
└─plugins      　   　   # 放置自定義的 plugin
```

## 建立 Dag
- Dag 檔案需要放置在資料夾 `dags` 底下

```py
# first_dag.py
from airflow import DAG
from airflow.operators.bash import BashOperator  # BashOperator：執行 shell script
from airflow.operators.empty import EmptyOperator  # EmptyOperator：一個空的流程，常用來製作開頭 or 結束
from datetime import datetime, timedelta

# default setting
default_args = {
    "owner": "mandy",  # 擁有者
    "depends_on_past": False,
    "start_date": datetime(2023, 5, 25),  # 該 dag 開始的執行時間(可以是過去的時間，會從過去的時間點開始執行)
    "email": ["ms.mandy610425@gmail.com"],
    "email_on_failure": False,
    "email_on_retry": False,
    "retries": 1,  # 執行失敗時，要重試多少次
    "retry_delay": timedelta(minutes=5),  # 執行失敗時，過多久才重試
    "schedule_interval": "0 0 * * *",
}

# 建立一個名為 first_dag 的 dag ，並且指派 tags 屬性，方便我們在 UI 上進行查詢
dag = DAG(
    dag_id="first_dag",
    default_args=default_args,
    tags=["user"],
    description="A simple tutorial DAG",
)
with dag:
    start_task = EmptyOperator(task_id="start_task")
    end_task = EmptyOperator(task_id="end_task")
    first_task = BashOperator(
        task_id="first_task", bash_command=f"echo execute time: {datetime.now()}"
    )
    # 透過 >> 符號來將各個 Operator 進行串聯
    start_task >> first_task >> end_task

```

## 查看 Dag
- 到 UI 點選 `DAGs` 
- 選取 Dag 所屬的 `tags`：`user`

![airflow dag tag filter.png](http://192.168.25.60:8000/files/file_storage/50bd50db.png)

- 點選 `first_dag` 進入到 Dag 中
- 選取 `Gragh` 查看 task 串聯

![airflow dag example first_dag graph.png](http://192.168.25.60:8000/files/file_storage/8457a4be.png)

## 執行 Dag
- 透過 UI 將 Dag 設定為 `Unpause` 狀態
- 點選右方三角形的執行符號，選擇 `Trigger DAG` 選項

![airflow trigger dag manually.png](http://192.168.25.60:8000/files/file_storage/f920d742.png)

- 之後就可以在 Runs 欄位看到該 Dag 的運行狀態，從左至右分別為 `queued`、`success`、`running` 以及 `failed`，而上面的數字代表著數量

# 實作自定義 Operator
```
│  .env                                       # 環境變數檔
│  .gitignore                                 # git 忽略檔案
│  docker-compose.yaml                        # docker compose 設定檔
│  dockerfile                                 # dockerfile
│  README.md                                  # 專案說明檔案
│  requirements.txt                           # 安裝套件清單
│  jtds-1.3.1.jar      　                     # JDBC Driver
│
├─config      　   　   　                     # 放置自定義的 log 參數
├─dags      　   　   　　                     # 放置 dag 檔案
│   ├─A_SY004     　   　                     # 依照 db 分類的 dag 資料夾
│   │      acpta_dag.py                      ## 依照 table 命名的 dag 檔案
│   ├─models                                 # 放置 model 檔案
│   │      A_SY004.py                        ## 依照 db 分類的 model 資料夾
│   └─operators                              # 放置自定義的 operator 檔案
│          mssql_to_postgres_operator.py     ## 從 mssql 到 postgres 的 operator
│    
├─logs      　   　   　　                     # 包含來自任務執行和排程的日誌
└─plugins      　   　                        # 放置自定義的 plugin
```

## 建立 Operator
- 將自定義的 Operator 檔案放置在資料夾 `operators` 底下
- 使用 `JdbcHook` 連接 mssql
- 使用 `PostgresHook` 連接 postgres
- `execute` 邏輯：從資料來源撈取資料，寫入資料目的
```py
# mssql_to_postgres_operator.py
from airflow.hooks.base import BaseHook
from airflow.providers.jdbc.hooks.jdbc import JdbcHook
from airflow.providers.postgres.hooks.postgres import PostgresHook
from airflow.models.baseoperator import BaseOperator
from airflow.utils.decorators import apply_defaults


class MsSqlToPostgresOperator(BaseOperator):
    @apply_defaults
    def __init__(
        self,
        sql=None,
        target_table=None,
        mssql_conn_id="local_sql_server_conn",
        postgres_conn_id="local_postgres_conn",
        *args,
        **kwargs
    ):
        super().__init__(*args, **kwargs)
        self.sql = sql
        self.target_table = target_table
        self.mssql_conn_id = mssql_conn_id
        self.postgres_conn_id = postgres_conn_id

    def execute(self, context):
        print("sql", self.sql)
        # 資料來源
        source = JdbcHook(self.mssql_conn_id)
        # 資料目的
        target = PostgresHook(self.postgres_conn_id)
        # 資料來源取得連線
        conn = source.get_conn()
        cursor = conn.cursor()
        # 執行查詢 sql
        self.sql = "select * from " + self.target_table + ";"
        cursor.execute(self.sql)
        # 取得資料來源 table 欄位名稱
        target_fields = ['"' + str(x[0]) + '"' for x in cursor.description]
        # 取得資料來源 table 內的資料
        rows = cursor.fetchall()
        # 資料目的 table
        self.target_table = '"public".' + '"' + self.target_table + '"'
        # 資料目的 table 寫入資料
        try:
            target.insert_rows(
                self.target_table,
                rows,
                target_fields=target_fields,
            )
            print("insert rows into", self.target_table + "successfully")
        except Exception as e:
            print("insert rows into", self.target_table + "failed" + "：" + e)
        cursor.close()
        conn.close()

```
## 建立 Model
- 將各 db 的 model 檔案放置在資料夾 `models` 底下
```py
# A_SY004.py
class A_SY004:
    ACPMA = "ACPMA"
    ACPMB = "ACPMB"
    ACPTA = "ACPTA"
    ACPTB = "ACPTB"
    ACPTC = "ACPTC"
    ACPTD = "ACPTD"
    ACPTE = "ACPTE"
    ACPTF = "ACPTF"
    ACRMA = "ACRMA"
    ACRTA = "ACRTA"
    ACRTB = "ACRTB"
    ACRTC = "ACRTC"
    ACRTD = "ACRTD"
    ACRTE = "ACRTE"
```
## 建立 Dag
- 將各 db 的 dag 依照資料夾分類放置
- 使用自定義的 Operator `MsSqlToPostgresOperator`
- `tags`：依照所屬 db 建立

```py
# acpta_dag.py
from datetime import datetime, timedelta
from models.A_SY004 import A_SY004
from airflow import DAG
from operators.mssql_to_postgres_operator import MsSqlToPostgresOperator
from airflow.operators.empty import EmptyOperator


class Settings:
    table = A_SY004.ACPTA
    tags = ["A_SY004"]
    start_task_id = "start_task"
    end_task_id = "end_task"


# default setting
default_args = {
    "owner": "mandy",  # 擁有者
    "depends_on_past": False,
    "start_date": datetime(2023, 7, 1),  # 該 dag 開始的執行時間(可以是過去的時間，會從過去的時間點開始執行)
    "email": ["mandy.chen@hokia.com.tw"],
    "email_on_failure": True,
    "email_on_retry": False,
    "retries": 1,  # 執行失敗時，要重試多少次
    "retry_delay": timedelta(minutes=5),  # 執行失敗時，過多久才重試
    "schedule_interval": "0 0 * * *",
}

dag = DAG(
    dag_id=Settings.table + "_DAG",
    default_args=default_args,
    tags=Settings.tags,
    concurrency=100,
)

start = MsSqlToPostgresOperator(
    task_id=Settings.table,
    target_table=Settings.table,
    dag=dag,
)

start_task = EmptyOperator(
    task_id=Settings.start_task_id,
    default_args=default_args,
)
end_task = EmptyOperator(
    task_id=Settings.end_task_id,
    default_args=default_args,
)

start_task >> start >> end_task

```

## 查看 Dag
- 到 UI 點選 `DAGs` 
- 選取 Dag 所屬的 `tags`：`A_SY004`
- 點選 `acpta_dag` 進入到 Dag 中
- 選取 `Gragh` 查看 task 串聯

![airflow dag example ACPTA_DAG graph.png](http://192.168.25.60:8000/files/file_storage/e7736873.png)

## 查看 Log
- 點選 `task`，右方點選 `Logs` 查看 Log

![airflow dag excute log.png](http://192.168.25.60:8000/files/file_storage/4fb873c5.png)

- 批次寫入 1000 筆，一次大約 2-3 分鐘
- 顯示總共寫入幾筆資料
```
[2023-07-02, 08:04:40 CST] {sql.py:470} INFO - Loaded 82000 rows into "public"."ACPTA" so far
[2023-07-02, 08:04:43 CST] {sql.py:470} INFO - Loaded 83000 rows into "public"."ACPTA" so far
[2023-07-02, 08:04:45 CST] {sql.py:473} INFO - Done loading. Loaded a total of 83686 rows into "public"."ACPTA"
[2023-07-02, 08:04:45 CST] {logging_mixin.py:149} INFO - insert rows into "public"."ACPTA"successfully
```





