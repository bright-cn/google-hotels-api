# 谷歌酒店抓取器

[![促销](https://github.com/bright-cn/LinkedIn-Scraper/blob/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/products/serp-api/google-search/hotels)

学习如何从 Google（全球最大的旅行数据聚合平台之一）抓取实时酒店数据。本文涵盖两种方式：

1. 免费抓取器：适合小规模需求
2. Bright Data 谷歌酒店 API：企业级方案，基于单次 API 调用即可大规模采集公开的 Google Hotels 数据（隶属 [SERP Scraping API](https://www.bright.cn/products/serp-api)）

## 目录
- [免费抓取器](#免费抓取器)
  - [设置](#设置)
  - [用法](#用法)
  - [示例输出](#示例输出)
  - [限制](#限制)
- [Bright Data 谷歌酒店 API](#bright-data-谷歌酒店-api)
  - [主要特性](#主要特性)
  - [前置条件](#前置条件)
  - [直接调用 API](#直接调用-api)
  - [原生代理接入](#原生代理接入)
- [高级参数](#高级参数)
  - [本地化参数](#本地化参数)
  - [预订参数](#预订参数)
  - [设备类型参数](#设备类型参数)
  - [响应格式](#响应格式)
- [替代方案](#替代方案)
- [支持与资源](#支持与资源)

## 免费抓取器

一款快速易用的抓取器，用于小规模提取 Google Hotels 数据。

<img width="800" alt="免费谷歌酒店抓取器" src="https://github.com/bright-cn/google-hotels-api/blob/main/images/421713152-9e86aabe-c8b7-4286-946a-378cd98c81b3.png" />

### 设置

要求：

- [Python 3.9+](https://www.python.org/downloads/)
- 用于浏览器自动化的 [Selenium](https://pypi.org/project/selenium/)
- 用于解析 HTML 的 [BeautifulSoup](https://pypi.org/project/beautifulsoup4/)
- 其他辅助库：`pandas`、`tqdm`、`webdriver-manager`

安装：

```bash
pip install pandas tqdm selenium beautifulsoup4 webdriver-manager
```

注意：如果您是爬虫新手，建议先阅读我们的[Python 爬虫入门教程](https://www.bright.cn/blog/how-tos/web-scraping-with-python)或[使用 Selenium 进行网页抓取指南](https://www.bright.cn/blog/how-tos/using-selenium-for-web-scraping)。

### 用法
使用所需参数运行 [google-hotels-scraper.py](https://github.com/bright-cn/google-hotels-api/blob/main/google-hotels-scraper/google-hotels-scraper.py) 脚本：
```bash
python3 google-hotels-scraper.py --location "Dubai" --max_hotels 200
```
参数说明：
- `location` – 目标酒店数据的地理位置
- `max_hotels` – 抓取的酒店数量上限

小提示：为降低被 Google 反爬系统识别的概率，可注释掉脚本中的 `options.add_argument("--headless=new")` 这一行。

### 示例输出
<img width="800" alt="谷歌酒店抓取器 CSV 输出" src="https://github.com/bright-cn/google-hotels-api/blob/main/images/421731827-633afbf9-204e-444a-ac0f-23b8b72c5813.png" />

### 限制

免费抓取器存在以下限制：

- 高 IP 封禁风险
- 请求量受限
- 频繁触发验证码（CAPTCHA）
- 大规模抓取不够稳定

如需更大规模、更稳定的数据采集，请参考下方的官方解决方案 👇

## Bright Data 谷歌酒店 API
[Bright Data 的 Google Hotels API](https://www.bright.cn/products/serp-api/google-search/hotels) 属于 [SERP Scraping API](https://www.bright.cn/products/serp-api) 组件，基于我们先进的[代理网络](https://www.bright.cn/proxy-types)。无需担心验证码或 IP 封禁，即可大规模采集公开的 Google Hotels 数据。

### 主要特性
- 全球定位精准：可按指定位置定制结果
- 成功计费模式：仅对成功请求计费
- 实时数据：秒级获取最新酒店信息
- 可扩展性：无限请求量，无并发限制
- 成本高效：节省基础设施与维护成本
- 高可靠性：内置防封策略，表现稳定
- 7x24 支持：随时获得专家协助

### 前置条件

1. 创建一个 [Bright Data 账号](https://www.bright.cn/)（新用户赠送 $5 额度）
2. 生成[API 密钥](https://docs.brightdata.com/general/account/api-token)
3. 按照[分步指南](https://github.com/bright-cn/google-hotels-api/blob/main/setup-serp-api-guide.md)配置 SERP API 与访问凭据
4. 使用 Google Hotels API 需要查询酒店的 entity ID，获取方法：
  1. 在 Google 中搜索酒店名称
  2. 右键页面并选择“查看网页源代码”
  3. 在源代码中搜索 “/entity” 即可找到 entity ID

### 直接调用 API

向 API 端点发起请求。

cURL 示例：

```bash
curl https://api.brightdata.com/request \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer API_TOKEN" \
  -d '{
        "zone": "ZONE_NAME",
        "url": "https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_json=1",
        "format": "raw"
      }'
```

Python 示例：

```python
import requests
import json

url = "https://api.brightdata.com/request"
headers = {"Content-Type": "application/json", "Authorization": "Bearer API_TOKEN"}

payload = {
    "zone": "ZONE_NAME",
    "url": "https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_json=1",
    "format": "raw",
}

response = requests.post(url, headers=headers, json=payload)

with open("serp-direct-api.json", "w") as file:
    json.dump(response.json(), file, indent=4)

print("Response saved to 'serp-direct-api.json'.")
```

👉 查看[完整 JSON 输出](https://github.com/bright-cn/google-hotels-api/blob/main/google-hotels-api-results/serp-direct-api.json)。

注意：使用 `brd_json=1` 返回解析后的 JSON；使用 `brd_json=html` 返回“解析后的 JSON + 完整嵌套 HTML”。

### 原生代理接入

也可使用 Bright Data 的代理路由方式：

cURL 示例：

```bash
curl -i \
  --proxy brd.superproxy.io:33335 \
  --proxy-user "brd-customer-<customer-id>-zone-<zone-name>:<zone-password>" \
  -k \
  "https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_json=html"
```

Python 示例：

```python
import requests
import urllib3

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

host = "brd.superproxy.io"
port = 33335
username = "brd-customer-<customer-id>-zone-<zone-name>"
password = "<zone-password>"
proxy_url = f"http://{username}:{password}@{host}:{port}"

proxies = {"http": proxy_url, "https": proxy_url}
url = "https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_json=html"
response = requests.get(url, proxies=proxies, verify=False)

with open("serp-native-proxy.html", "w", encoding="utf-8") as file:
    file.write(response.text)

print("Response saved to 'serp-native-proxy.html'.")
```

👉 查看[完整输出](https://github.com/bright-cn/Google-Hotels-API/blob/main/google-hotels-api-results/serp-native-proxy.html)。

注意：生产环境请按[SSL 证书指南](https://docs.brightdata.com/general/account/ssl-certificate)加载 Bright Data 的 SSL 证书。

## 高级参数

Bright Data 的 API 支持多种高级参数，用于精准优化 Google Hotels 数据的抓取。以下示例基于“原生代理接入”，同样适用于“直接调用 API”。

### 本地化参数

<img width="800" alt="Bright Data 谷歌酒店 API 本地化参数" src="https://github.com/bright-cn/google-hotels-api/blob/main/images/422299775-d47254c1-0c7f-4572-bf54-f3f55cf66908.png" />

这些参数用于设置搜索的国家与语言：

| 参数 | 说明 | 示例 |
| --- | --- | --- |
| gl | 两位国家代码 | `gl=us`（美国） |
| hl | 两位语言代码 | `hl=en`（英文） |

示例：在美国搜索，并以英文返回结果：

```bash
curl --proxy brd.superproxy.io:33335 --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
"https://www.google.com/travel/hotels/entity/CgoI4NzJmsPmkpU6EAE/prices?gl=us&hl=en"
```

### 预订参数

<img width="800" alt="Bright Data 谷歌酒店 API 预订参数" src="https://github.com/bright-cn/google-hotels-api/blob/main/images/422303757-74faadf7-218b-4fa3-b2d9-d0cecf8e23e6.png" />

用于按日期、入住人数、可退款与住宿类型等条件筛选结果：

| 参数 | 说明 | 格式 | 示例 |
|-----------|-------------|--------|---------|
| brd_dates | 入住与退房日期 | YYYY-MM-DD,YYYY-MM-DD | `brd_dates=2025-08-15,2025-08-20` |
| brd_occupancy | 入住人数（成人+儿童） | `<adults>,<child1_age>,<child2_age>` | `brd_occupancy=3,6,9`（3 位成人，2 名儿童年龄 6 和 9） |
| brd_free_cancellation | 仅显示可免费取消 | true 或 false | `brd_free_cancellation=true` |
| brd_accomodation_type | 住宿类型 | hotels 或 vacation_rentals | `brd_accomodation_type=vacation_rentals` |
| brd_currency | 价格货币 | 三位货币代码 | `brd_currency=GBP`（英镑） |

示例：按指定参数搜索入住：

```bash
curl --proxy brd.superproxy.io:33335 \
  --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
  "https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices\
?brd_dates=2025-04-15,2025-04-20\
&brd_occupancy=3,6,9\
&brd_free_cancellation=true\
&brd_currency=GBP"
```

### 设备类型参数
默认使用桌面端 UA（User-Agent），也可切换为移动端：

| 参数 | 说明 |
|-----------|-------------|
| `brd_mobile=0` | 随机桌面端 UA（默认） |
| `brd_mobile=1` | 随机移动端 UA |
| `brd_mobile=ios` | iPhone UA |
| `brd_mobile=android` | Android 手机 UA |
| `brd_mobile=ipad` | iPad UA |
| `brd_mobile=android_tablet` | Android 平板 UA |

示例：使用 Android 手机 UA 获取酒店数据：

```bash
curl --proxy brd.superproxy.io:33335 --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
"https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_mobile=android"
```

### 响应格式
默认返回 HTML，可按需请求 JSON：

| 参数 | 说明 |
|-----------|-------------|
| `brd_json=1` | 返回 JSON（不含嵌套 HTML） |
| `brd_json=html` | 返回“JSON + 完整嵌套 HTML” |

示例：以 JSON 格式获取酒店价格数据：

```bash
curl --proxy brd.superproxy.io:33335 --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
"https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_json=1"
```

## 替代方案

除[Web Scraper APIs](https://www.bright.cn/products/web-scraper) 外，Bright Data 还提供面向旅游行业的即用型[数据集](https://www.bright.cn/products/datasets)：

- [酒店数据集](https://www.bright.cn/products/datasets/travel/hotels)
- [Airbnb 数据集](https://www.bright.cn/products/datasets/airbnb)
- [Expedia 数据集](https://www.bright.cn/products/datasets/travel/expedia)
- [旅游数据集](https://www.bright.cn/products/datasets/tourism)
- [Booking.com 数据集](https://www.bright.cn/products/datasets/booking)
- [TripAdvisor 数据集](https://www.bright.cn/products/datasets/tripadvisor)

## 支持与资源

- 文档：[SERP API 文档](https://docs.brightdata.com/scraping-automation/serp-api/)
- 相关指南：
  - [Web Unlocker API](https://github.com/bright-cn/web-unlocker-api)
  - [SERP API](https://github.com/bright-cn/serp-api)
  - [Google Search API](https://github.com/bright-cn/google-search-api)
  - [Google News Scraper](https://github.com/bright-cn/Google-News-Scraper)
  - [Google Trends API](https://github.com/bright-cn/google-trends-api)
- 实用文章：
  - [最佳 SERP API 盘点](https://www.bright.cn/blog/web-data/best-serp-apis)
  - [用 SERP API 构建 RAG 聊天机器人](https://www.bright.cn/blog/web-data/build-a-rag-chatbot)
- 使用场景：
  - [SEO 应用](https://www.bright.cn/use-cases/serp-tracking)
  - [旅游行业](https://www.bright.cn/use-cases/travel)
- 联系我们：需要帮助？请发送邮件至 support@brightdata.com
