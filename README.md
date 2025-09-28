# Google 酒店抓取器

[![推广](https://github.com/bright-cn/LinkedIn-Scraper/blob/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://www.bright.cn/products/serp-api/google-search/hotels)

了解如何从 Google（全球最大的旅行数据聚合平台之一）抓取实时酒店数据。我们提供两种方法：

1. 免费抓取器：适合小规模需求
2. Bright Data Google Hotels API：企业级方案，基于一次 API 调用即可规模化采集公开的 Google Hotels 数据（属于 [SERP Scraping API](https://www.bright.cn/products/serp-api) 的一部分）

## 目录
- [Free Scraper](#free-scraper)
  - [Setup](#setup)
  - [Usage](#usage)
  - [Sample Output](#sample-output)
  - [Limitations](#limitations)
- [Bright Data Google Hotels API](#bright-data-google-hotels-api)
  - [Key Features](#key-features)
  - [Prerequisites](#prerequisites)
  - [Direct API Access](#direct-api-access)
  - [Native Proxy-Based Access](#native-proxy-based-access)
- [Advanced Features](#advanced-features)
  - [Localization Parameters](#localization-parameters)
  - [Booking Parameters](#booking-parameters)
  - [Device Type Parameters](#device-type-parameters)
  - [Response Format](#response-format)
- [Alternative Solutions](#alternative-solutions)
- [Support & Resources](#support--resources)

## Free Scraper

一个用于小规模提取 Google Hotels 数据的快速简易抓取器。

<img width="800" alt="free-google-hotels-scraper" src="https://github.com/bright-cn/google-hotels-api/blob/main/images/421713152-9e86aabe-c8b7-4286-946a-378cd98c81b3.png" />

### Setup

要求：

- [Python 3.9+](https://www.python.org/downloads/)
- 用于浏览器自动化的 [Selenium](https://pypi.org/project/selenium/)
- 用于解析 HTML 的 [BeautifulSoup](https://pypi.org/project/beautifulsoup4/)
- 其他辅助库，如 `pandas`、`tqdm`、`webdriver-manager`

安装：

```bash
pip install pandas tqdm selenium beautifulsoup4 webdriver-manager
```

注意：如果你是爬取新手，建议先阅读我们的[Python 爬取新手教程](https://www.bright.cn/blog/how-tos/web-scraping-with-python)或[使用 Selenium 进行网页爬取指南](https://www.bright.cn/blog/how-tos/using-selenium-for-web-scraping)。

### Usage
使用必需的参数运行 [google-hotels-scraper.py](https://github.com/bright-cn/google-hotels-api/blob/main/google-hotels-scraper/google-hotels-scraper.py) 脚本：
```bash
python3 google-hotels-scraper.py --location "Dubai" --max_hotels 200
```
参数：
- `location` —— 目标酒店数据的地点
- `max_hotels` —— 要抓取的酒店数量

💡 专业提示：在脚本中注释掉 `options.add_argument("--headless=new")` 这一行，可降低被 Google 反爬系统检测到的风险。

### Sample Output
<img width="800" alt="google-hotels-scraper-csv-output" src="https://github.com/bright-cn/google-hotels-api/blob/main/images/421731827-633afbf9-204e-444a-ac0f-23b8b72c5813.png" />

### Limitations

免费抓取器存在以下限制：

- IP 被封风险高
- 请求量有限
- 频繁触发验证码（CAPTCHA）
- 不适合大规模抓取，稳定性较差

如需更大规模、更可靠的数据采集，请考虑下方 Bright Data 的专用方案👇

## Bright Data Google Hotels API
[Bright Data 的 Google Hotels API](https://www.bright.cn/products/serp-api/google-search/hotels) 属于 [SERP Scraping API](https://www.bright.cn/products/serp-api) 的一部分，基于我们先进的[代理网络](https://www.bright.cn/proxy-types)。它可帮助你规模化采集公开的 Google Hotels 数据，无需担心验证码或 IP 封禁问题。

### Key Features
- 全球定位精准：可针对特定地点定制结果
- 按成功计费：仅为成功请求付费
- 实时数据：数秒获取最新酒店信息
- 可扩展性强：无限请求，无量级限制
- 成本更优：节省基础设施与维护成本
- 高可靠性：内置反封锁机制，表现稳定
- 7×24 小时支持：需要时随时获得专家帮助

### Prerequisites

1. 创建一个 [Bright Data 账号](https://www.bright.cn/)（新用户可获 $5 额度）
2. 生成你的 [API Key](https://docs.brightdata.com/general/account/api-token)
3. 按照我们的[分步指南](https://github.com/bright-cn/google-hotels-api/blob/main/setup-serp-api-guide.md)配置 SERP API 和访问凭据
4. 使用 Google Hotels API 需要你要查询的酒店的 entity ID。查找方式：
  1. 在 Google 中搜索该酒店名称
  2. 右键页面并选择“查看页面源代码”
  3. 在源代码中搜索 “/entity” 即可找到 entity ID

### Direct API Access

直接调用 API 接口。

cURL 示例：

```bash
curl https://api.brightdata.com/request \
-H "Content-Type: application/json" \
-H "Authorization: Bearer API_TOKEN" \
-d '{
zone: "ZONE_NAME",
url: "https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_json=1",
format: "raw"
}'
```

Python 示例：

```python
import requests
import json

url = "https://api.brightdata.com/request"
headers = {"Content-Type": "application/json", "Authorization": "Bearer API_TOKEN"}

payload = {
zone: "ZONE_NAME",
url: "https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_json=1",
format: "raw",
}

response = requests.post(url, headers=headers, json=payload)

with open("serp-direct-api.json", "w") as file:
json.dump(response.json(), file, indent=4)

print("Response saved to 'serp-direct-api.json'.")
```

👉 查看[完整 JSON 输出](https://github.com/bright-cn/google-hotels-api/blob/main/google-hotels-api-results/serp-direct-api.json)。

注意：使用 `brd_json=1` 获取解析后的 JSON，或使用 `brd_json=html` 获取“解析后的 JSON + 完整嵌套 HTML”。

### Native Proxy-Based Access

你也可以使用 Bright Data 的原生代理路由方式：

cURL 示例：

```bash
curl -i \
--proxy brd.superproxy.io:33335 \
--proxy-user "brd-customer-<customer-id>-zone-<zone-name>:<zone-password>" \
-k \
https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_json=html
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

👉 查看[完整 JSON 输出](https://github.com/bright-cn/Google-Hotels-API/blob/main/google-hotels-api-results/serp-native-proxy.html)。

注意：生产环境中，请按[SSL 证书指南](https://docs.brightdata.com/general/account/ssl-certificate)加载 Bright Data 的 SSL 证书。

## Advanced Features

Bright Data 的 API 支持大量高级参数，帮助你更精细地抓取 Google Hotels 数据。下面示例使用“原生代理路由方式”，也可在“直接 API 调用”中使用同样的参数。

### Localization Parameters

<img width="800" alt="bright-data-google-hotels-scraper-api-localization" src="https://github.com/bright-cn/google-hotels-api/blob/main/images/422299775-d47254c1-0c7f-4572-bf54-f3f55cf66908.png" />

以下参数用于定义搜索的国家和语言：

| 参数 | 描述 | 示例 |
| --- | --- | --- |
| gl | 两位国家代码 | `gl=us`（美国） |
| hl | 两位语言代码 | `hl=en`（英语） |

示例：在美国搜索酒店并以英文返回结果：

```bash
curl --proxy brd.superproxy.io:33335 --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
https://www.google.com/travel/hotels/entity/CgoI4NzJmsPmkpU6EAE/prices?gl=us&hl=en
```

### Booking Parameters

<img width="800" alt="bright-data-google-hotels-scraper-api-booking-params" src="https://github.com/bright-cn/google-hotels-api/blob/main/images/422303757-74faadf7-218b-4fa3-b2d9-d0cecf8e23e6.png" />

这些参数可按日期、入住人数、是否可免费取消、住宿类型等细化结果：

| 参数 | 描述 | 格式 | 示例 |
|-----------|-------------|--------|---------|
| brd_dates | 入住与退房日期 | YYYY-MM-DD,YYYY-MM-DD | `brd_dates=2025-08-15,2025-08-20` |
| brd_occupancy | 入住人数（成人 + 儿童） | `<adults>,<child1_age>,<child2_age>` | `brd_occupancy=3,6,9`（3 位成人，2 名儿童，年龄 6 与 9） |
| brd_free_cancellation | 仅显示可退款预订 | true 或 false | `brd_free_cancellation=true` |
| brd_accomodation_type | 住宿类型 | hotels 或 vacation_rentals | `brd_accomodation_type=vacation_rentals` |
| brd_currency | 价格显示货币 | 三位货币代码 | `brd_currency=GBP`（英镑） |

示例：按指定参数搜索酒店住宿：
```bash
curl --proxy brd.superproxy.io:33335 \
--proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
"https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices\
?brd_dates=2025-04-15,2025-04-20\
&brd_occupancy=3,6,9\
&brd_free_cancellation=true\
&brd_currency=GBP"
```

### Device Type Parameters
默认情况下，请求会模拟桌面端 User-Agent，你也可以切换为移动端：

| 参数 | 描述 |
|-----------|-------------|
| `brd_mobile=0` | 随机桌面端 User-Agent（默认） |
| `brd_mobile=1` | 随机移动端 User-Agent |
| `brd_mobile=ios` | iPhone User-Agent |
| `brd_mobile=android` | Android 手机 User-Agent |
| `brd_mobile=ipad` | iPad User-Agent |
| `brd_mobile=android_tablet` | Android 平板 User-Agent |

示例：使用 Android 手机 User-Agent 抓取酒店数据：

```bash
curl --proxy brd.superproxy.io:33335 --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_mobile=android
```

### Response Format
默认返回 HTML，你也可以请求 JSON：

| 参数 | 描述 |
|-----------|-------------|
| `brd_json=1` | 返回 JSON（替代 HTML） |
| `brd_json=html` | 返回 JSON + 完整嵌套 HTML |

示例：以 JSON 格式获取酒店价格数据：

```bash
curl --proxy brd.superproxy.io:33335 --proxy-user brd-customer-<customer-id>-zone-<zone-name>:<zone-password> \
https://www.google.com/travel/hotels/entity/CgoIyNaqqL33x5ovEAE/prices?brd_json=1
```

## Alternative Solutions

除[网页抓取 API](https://www.bright.cn/products/web-scraper) 外，Bright Data 还提供面向旅游行业的即用型[数据集](https://www.bright.cn/products/datasets)：

- [酒店数据集](https://www.bright.cn/products/datasets/travel/hotels)
- [Airbnb 数据集](https://www.bright.cn/products/datasets/airbnb)
- [Expedia 数据集](https://www.bright.cn/products/datasets/travel/expedia)
- [旅游数据集](https://www.bright.cn/products/datasets/tourism)
- [Booking.com 数据集](https://www.bright.cn/products/datasets/booking)
- [TripAdvisor 数据集](https://www.bright.cn/products/datasets/tripadvisor)

## Support & Resources

- 文档：[SERP API 文档](https://docs.brightdata.com/scraping-automation/serp-api/)
- 相关指南：
- [Web Unlocker API](https://github.com/bright-cn/web-unlocker-api)
- [SERP API](https://github.com/bright-cn/serp-api)
- [Google Search API](https://github.com/bright-cn/google-search-api)
- [Google News Scraper](https://github.com/bright-cn/Google-News-Scraper)
- [Google Trends API](https://github.com/bright-cn/google-trends-api)
- 实用文章：
- [最佳 SERP API](https://www.bright.cn/blog/web-data/best-serp-apis)
- [用 SERP API 构建 RAG 聊天机器人](https://www.bright.cn/blog/web-data/build-a-rag-chatbot)
- 应用场景：
- [SEO 应用](https://www.bright.cn/use-cases/serp-tracking)
- [旅游行业应用](https://www.bright.cn/use-cases/travel)
- 联系方式：需要帮助？请发送邮件至 support@brightdata.com
