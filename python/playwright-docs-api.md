# Playwright-docs-api 文档全文拷贝

[Playwright | Playwright Python 官方文档--需要科学上网](https://playwright.dev/python/docs/api/class-playwright)

vesion: 1.20

language: Python

拷贝时间: 2022.03.31

拷贝完整度: > 99%

备注: 该文档针对 typora 的使用方式做了部分优化(超链接显示,锚点跳转[具体优化方式](./playwright文档优化方式.md))

# Playwright

Playwright module provides a method to launch a browser instance. The following is a typical example of using Playwright to drive automation:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    chromium = playwright.chromium # or "firefox" or "webkit".
    browser = chromium.launch()
    page = browser.new_page()
    page.goto("http://example.com")
    # other actions...
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    chromium = playwright.chromium # or "firefox" or "webkit".
    browser = await chromium.launch()
    page = await browser.new_page()
    await page.goto("http://example.com")
    # other actions...
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

- [playwright.stop()](#playwright-stop)
- [playwright.chromium](#playwright-chromium)
- [playwright.devices](#playwright-devices)
- [playwright.firefox](#playwright-firefox)
- [playwright.request](#playwright-request)
- [playwright.selectors](#playwright-selectors)
- [playwright.webkit](#playwright-webkit)

## playwright.stop()<a name="playwright-stop">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="playwright-stop-return">#</a>

Terminates this instance of Playwright in case it was created bypassing the Python context manager. This is useful in REPL applications.

```python
>>> from playwright.sync_api import sync_playwright

>>> playwright = sync_playwright().start()

>>> browser = playwright.chromium.launch()
>>> page = browser.new_page()
>>> page.goto("http://whatsmyuseragent.org/")
>>> page.screenshot(path="example.png")
>>> browser.close()

>>> playwright.stop()
```

## playwright.chromium<a name="playwright-chromium">#</a>

- type: \<[BrowserType](#browsertype)>

This object can be used to launch or connect to Chromium, returning instances of [Browser](#browser).

## playwright.devices<a name="playwright-devices">#</a>

- type: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)>

Returns a dictionary of devices to be used with [browser.new_context(**kwargs)](#browser-new-context) or [browser.new_page(**kwargs)](#browser-new-page).

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    webkit = playwright.webkit
    iphone = playwright.devices["iPhone 6"]
    browser = webkit.launch()
    context = browser.new_context(**iphone)
    page = context.new_page()
    page.goto("http://example.com")
    # other actions...
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    webkit = playwright.webkit
    iphone = playwright.devices["iPhone 6"]
    browser = await webkit.launch()
    context = await browser.new_context(**iphone)
    page = await context.new_page()
    await page.goto("http://example.com")
    # other actions...
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

## playwright.firefox<a name="playwright-firefox">#</a>

- type: \<[BrowserType](#browsertype)>

This object can be used to launch or connect to Firefox, returning instances of [Browser](#browser).

## playwright.request<a name="playwright-request">#</a>

- type: \<[APIRequest](#apirequest)>

Exposes API that can be used for the Web API testing.

## playwright.selectors<a name="playwright-selectors">#</a>

- type: \<[Selectors](#selectors)>

Selectors can be used to install custom selector engines. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more information.

## playwright.webkit<a name="playwright-webkit">#</a>

- type: \<[BrowserType](#browsertype)>

This object can be used to launch or connect to WebKit, returning instances of [Browser](#browser).



# APIRequest

Exposes API that can be used for the Web API testing. Each Playwright browser context has a APIRequestContext instance attached which shares cookies with the page context. Its also possible to create a new APIRequestContext instance manually. For more information see [here](#apirequestcontext).

- [api_request.new_context(**kwargs)](#api-request-new-context)

## api_request.new_context(**kwargs)<a name="api-request-new-context">#</a>

- `base_url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Methods like [api_request_context.get(url, **kwargs)](#api-request-context-get) take the base URL into consideration by using the [`URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) constructor for building the corresponding URL. Examples:<a name="api-request-new-context-option-base-url">#</a>
    - baseURL: `http://localhost:3000` and sending request to `/bar.html` results in `http://localhost:3000/bar.html`
    - baseURL: `http://localhost:3000/foo/` and sending request to `./bar.html` results in `http://localhost:3000/foo/bar.html`
    - baseURL: `http://localhost:3000/foo` (without trailing slash) and navigating to `./bar.html` results in `http://localhost:3000/bar.html`
- `extra_http_headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> An object containing additional HTTP headers to be sent with every request.<a name="api-request-new-context-option-extra-http-headers">#</a>
- `http_credentials` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Credentials for [HTTP authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication).<a name="api-request-new-context-option-http-credentials">#</a>
    - `username` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `password` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="api-request-new-context-option-ignore-https-errors">#</a>
- `proxy` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Network proxy settings.<a name="api-request-new-context-option-proxy">#</a>
    - `server` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Proxy to be used for all requests. HTTP and SOCKS proxies are supported, for example `http://myproxy.com:3128` or `socks5://myproxy.com:3128`. Short form `myproxy.com:3128` is considered an HTTP proxy.
    - `bypass` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional comma-separated domains to bypass proxy, for example `".com, chromium.org, .domain.com"`.
    - `username` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional username to use if HTTP proxy requires authentication.
    - `password` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional password to use if HTTP proxy requires authentication.
- `storage_state` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Populates context with given storage state. This option can be used to initialize context with logged-in information obtained via [browser_context.storage_state(**kwargs)](#browser-context-storage-state) or [api_request_context.storage_state(**kwargs)](#api-request-context-storage-state). Either a path to the file with saved storage, or the value returned by one of [browser_context.storage_state(**kwargs)](#browser-context-storage-state) or [api_request_context.storage_state(**kwargs)](#api-request-context-storage-state) methods.<a name="api-request-new-context-option-storage-state">#</a>
    - `cookies` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
        - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `domain` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `path` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `expires` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Unix time in seconds.
        - `httpOnly` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `secure` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `sameSite` \<"Strict"|"Lax"|"None">
    - `origins` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
        - `origin` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `localStorage` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
            - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
            - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds to wait for the response. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="api-request-new-context-option-timeout">#</a>
- `user_agent` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Specific user agent to use in this context.<a name="api-request-new-context-option-user-agent">#</a>
- returns: \<[APIRequestContext](#apirequestcontext)><a name="api-request-new-context-return">#</a>

Creates new instances of [APIRequestContext](#apirequestcontext).



# APIRequestContext

This API is used for the Web API testing. You can use it to trigger API endpoints, configure micro-services, prepare environment or the service to your e2e test. When used on [Page](#page) or a [BrowserContext](#browsercontext), this API will automatically use the cookies from the corresponding [BrowserContext](#browsercontext). This means that if you log in using this API, your e2e test will be logged in and vice versa.

- Sync

```python
import os
from playwright.sync_api import sync_playwright

REPO = "test-repo-1"
USER = "github-username"
API_TOKEN = os.getenv("GITHUB_API_TOKEN")

with sync_playwright() as p:
    # This will launch a new browser, create a context and page. When making HTTP
    # requests with the internal APIRequestContext (e.g. `context.request` or `page.request`)
    # it will automatically set the cookies to the browser page and vise versa.
    browser = p.chromium.launch()
    context = browser.new_context(base_url="https://api.github.com")
    api_request_context = context.request
    page = context.new_page()

    # Alternatively you can create a APIRequestContext manually without having a browser context attached:
    # api_request_context = p.request.new_context(base_url="https://api.github.com")


    # Create a repository.
    response = api_request_context.post(
        "/user/repos",
        headers={
            "Accept": "application/vnd.github.v3+json",
            # Add GitHub personal access token.
            "Authorization": f"token {API_TOKEN}",
        },
        data={"name": REPO},
    )
    assert response.ok
    assert response.json()["name"] == REPO

    # Delete a repository.
    response = api_request_context.delete(
        f"/repos/{USER}/{REPO}",
        headers={
            "Accept": "application/vnd.github.v3+json",
            # Add GitHub personal access token.
            "Authorization": f"token {API_TOKEN}",
        },
    )
    assert response.ok
    assert await response.body() == '{"status": "ok"}'
```

- Async

```python
import os
import asyncio
from playwright.async_api import async_playwright, Playwright

REPO = "test-repo-1"
USER = "github-username"
API_TOKEN = os.getenv("GITHUB_API_TOKEN")

async def run(playwright: Playwright):
    # This will launch a new browser, create a context and page. When making HTTP
    # requests with the internal APIRequestContext (e.g. `context.request` or `page.request`)
    # it will automatically set the cookies to the browser page and vise versa.
    browser = await playwright.chromium.launch()
    context = await browser.new_context(base_url="https://api.github.com")
    api_request_context = context.request
    page = await context.new_page()

    # Alternatively you can create a APIRequestContext manually without having a browser context attached:
    # api_request_context = await playwright.request.new_context(base_url="https://api.github.com")

    # Create a repository.
    response = await api_request_context.post(
        "/user/repos",
        headers={
            "Accept": "application/vnd.github.v3+json",
            # Add GitHub personal access token.
            "Authorization": f"token {API_TOKEN}",
        },
        data={"name": REPO},
    )
    assert response.ok
    assert response.json()["name"] == REPO

    # Delete a repository.
    response = await api_request_context.delete(
        f"/repos/{USER}/{REPO}",
        headers={
            "Accept": "application/vnd.github.v3+json",
            # Add GitHub personal access token.
            "Authorization": f"token {API_TOKEN}",
        },
    )
    assert response.ok
    assert await response.body() == '{"status": "ok"}'

async def main():
    async with async_playwright() as playwright:
        await run(playwright)

asyncio.run(main())
```

## api_request_context.delete(url, **kwargs)<a name="api-request-context-delete">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Target URL.<a name="api-request-context-delete-option-url">#</a>
- `data` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)|[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)> Allows to set post data of the request. If the data parameter is an object, it will be serialized to json string and `content-type` header will be set to `application/json` if not explicitly set. Otherwise the `content-type` header will be set to `application/octet-stream` if not explicitly set.<a name="api-request-context-delete-option-data">#</a>
- `fail_on_status_code` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to throw on response codes other than 2xx and 3xx. By default response object is returned for all status codes.<a name="api-request-context-delete-option-fail-on-status-code">#</a>
- `form` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Provides an object that will be serialized as html form using `application/x-www-form-urlencoded` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `application/x-www-form-urlencoded` unless explicitly provided.<a name="api-request-context-delete-option-form">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Allows to set HTTP headers.<a name="api-request-context-delete-option-headers">#</a>
- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="api-request-context-delete-option-ignore-https-errors">#</a>
- `multipart` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)|[ReadStream]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> Provides an object that will be serialized as html form using `multipart/form-data` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `multipart/form-data` unless explicitly provided. File values can be passed either as [`fs.ReadStream`](https://nodejs.org/api/fs.html#fs_class_fs_readstream) or as file-like object containing file name, mime-type and its content.<a name="api-request-context-delete-option-multipart">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `params` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Query parameters to be sent with the URL.<a name="api-request-context-delete-option-params">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Request timeout in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="api-request-context-delete-option-timeout">#</a>
- returns: \<[APIResponse](#apiresponse)><a name="api-request-context-delete-return">#</a>

Sends HTTP(S) [DELETE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/DELETE) request and returns its response. The method will populate request cookies from the context and update context cookies from the response. The method will automatically follow redirects.

## api_request_context.dispose()<a name="api-request-context-dispose">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="api-request-context-dispose-return">#</a>

All responses returned by [api_request_context.get(url, **kwargs)](#api-request-context-get) and similar methods are stored in the memory, so that you can later call [api_response.body()](#api-response-body). This method discards all stored responses, and makes [api_response.body()](#api-response-body) throw "Response disposed" error.

## api_request_context.fetch(url_or_request, **kwargs)<a name="api-request-context-fetch">#</a>

- `url_or_request` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Request](#request)> Target URL or Request to get all parameters from.<a name="api-request-context-fetch-option-url-or-request">#</a>
- `data` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)|[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)> Allows to set post data of the request. If the data parameter is an object, it will be serialized to json string and `content-type` header will be set to `application/json` if not explicitly set. Otherwise the `content-type` header will be set to `application/octet-stream` if not explicitly set.<a name="api-request-context-fetch-option-data">#</a>
- `fail_on_status_code` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to throw on response codes other than 2xx and 3xx. By default response object is returned for all status codes.<a name="api-request-context-fetch-option-fail-on-status-code">#</a>
- `form` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Provides an object that will be serialized as html form using `application/x-www-form-urlencoded` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `application/x-www-form-urlencoded` unless explicitly provided.<a name="api-request-context-fetch-option-form">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Allows to set HTTP headers.<a name="api-request-context-fetch-option-headers">#</a>
- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="api-request-context-fetch-option-ignore-https-errors">#</a>
- `method` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> If set changes the fetch method (e.g. [PUT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT) or [POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST)). If not specified, GET method is used.<a name="api-request-context-fetch-option-method">#</a>
- `multipart` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)|[ReadStream]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> Provides an object that will be serialized as html form using `multipart/form-data` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `multipart/form-data` unless explicitly provided. File values can be passed either as [`fs.ReadStream`](https://nodejs.org/api/fs.html#fs_class_fs_readstream) or as file-like object containing file name, mime-type and its content.<a name="api-request-context-fetch-option-multipart">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `params` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Query parameters to be sent with the URL.<a name="api-request-context-fetch-option-params">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Request timeout in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="api-request-context-fetch-option-timeout">#</a>
- returns: \<[APIResponse](#apiresponse)><a name="api-request-context-fetch-return">#</a>

Sends HTTP(S) request and returns its response. The method will populate request cookies from the context and update context cookies from the response. The method will automatically follow redirects.

## api_request_context.get(url, **kwargs)<a name="api-request-context-get">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Target URL.<a name="api-request-context-get-option-url">#</a>
- `fail_on_status_code` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to throw on response codes other than 2xx and 3xx. By default response object is returned for all status codes.<a name="api-request-context-get-option-fail-on-status-code">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Allows to set HTTP headers.<a name="api-request-context-get-option-headers">#</a>
- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="api-request-context-get-option-ignore-https-errors">#</a>
- `params` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Query parameters to be sent with the URL.<a name="api-request-context-get-option-params">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Request timeout in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="api-request-context-get-option-timeout">#</a>
- returns: \<[APIResponse](#apiresponse)><a name="api-request-context-get-return">#</a>

Sends HTTP(S) [GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) request and returns its response. The method will populate request cookies from the context and update context cookies from the response. The method will automatically follow redirects.

## api_request_context.head(url, **kwargs)<a name="api-request-context-head">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Target URL.<a name="api-request-context-head-option-url">#</a>
- `fail_on_status_code` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to throw on response codes other than 2xx and 3xx. By default response object is returned for all status codes.<a name="api-request-context-head-option-fail-on-status-code">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Allows to set HTTP headers.<a name="api-request-context-head-option-headers">#</a>
- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="api-request-context-head-option-ignore-https-errors">#</a>
- `params` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Query parameters to be sent with the URL.<a name="api-request-context-head-option-params">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Request timeout in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="api-request-context-head-option-timeout">#</a>
- returns: \<[APIResponse](#apiresponse)><a name="api-request-context-head-return">#</a>

Sends HTTP(S) [HEAD](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD) request and returns its response. The method will populate request cookies from the context and update context cookies from the response. The method will automatically follow redirects.

## api_request_context.patch(url, **kwargs)<a name="api-request-context-patch">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Target URL.<a name="api-request-context-patch-option-url">#</a>
- `data` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)|[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)> Allows to set post data of the request. If the data parameter is an object, it will be serialized to json string and `content-type` header will be set to `application/json` if not explicitly set. Otherwise the `content-type` header will be set to `application/octet-stream` if not explicitly set.<a name="api-request-context-patch-option-data">#</a>
- `fail_on_status_code` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to throw on response codes other than 2xx and 3xx. By default response object is returned for all status codes.<a name="api-request-context-patch-option-fail-on-status-code">#</a>
- `form` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Provides an object that will be serialized as html form using `application/x-www-form-urlencoded` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `application/x-www-form-urlencoded` unless explicitly provided.<a name="api-request-context-patch-option-form">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Allows to set HTTP headers.<a name="api-request-context-patch-option-headers">#</a>
- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="api-request-context-patch-option-ignore-https-errors">#</a>
- `multipart` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)|[ReadStream]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> Provides an object that will be serialized as html form using `multipart/form-data` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `multipart/form-data` unless explicitly provided. File values can be passed either as [`fs.ReadStream`](https://nodejs.org/api/fs.html#fs_class_fs_readstream) or as file-like object containing file name, mime-type and its content.<a name="api-request-context-patch-option-multipart">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `params` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Query parameters to be sent with the URL.<a name="api-request-context-patch-option-params">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Request timeout in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="api-request-context-patch-option-timeout">#</a>
- returns: \<[APIResponse](#apiresponse)><a name="api-request-context-patch-return">#</a>

Sends HTTP(S) [PATCH](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH) request and returns its response. The method will populate request cookies from the context and update context cookies from the response. The method will automatically follow redirects.

## api_request_context.post(url, **kwargs)<a name="api-request-context-post">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Target URL.<a name="api-request-context-post-option-url">#</a>
- `data` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)|[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)> Allows to set post data of the request. If the data parameter is an object, it will be serialized to json string and `content-type` header will be set to `application/json` if not explicitly set. Otherwise the `content-type` header will be set to `application/octet-stream` if not explicitly set.<a name="api-request-context-post-option-data">#</a>
- `fail_on_status_code` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to throw on response codes other than 2xx and 3xx. By default response object is returned for all status codes.<a name="api-request-context-post-option-fail-on-status-code">#</a>
- `form` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Provides an object that will be serialized as html form using `application/x-www-form-urlencoded` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `application/x-www-form-urlencoded` unless explicitly provided.<a name="api-request-context-post-option-form">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Allows to set HTTP headers.<a name="api-request-context-post-option-headers">#</a>
- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="api-request-context-post-option-ignore-https-errors">#</a>
- `multipart` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)|[ReadStream]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> Provides an object that will be serialized as html form using `multipart/form-data` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `multipart/form-data` unless explicitly provided. File values can be passed either as [`fs.ReadStream`](https://nodejs.org/api/fs.html#fs_class_fs_readstream) or as file-like object containing file name, mime-type and its content.<a name="api-request-context-post-option-multipart">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `params` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Query parameters to be sent with the URL.<a name="api-request-context-post-option-params">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Request timeout in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="api-request-context-post-option-timeout">#</a>
- returns: \<[APIResponse](#apiresponse)><a name="api-request-context-post-return">#</a>

Sends HTTP(S) [POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) request and returns its response. The method will populate request cookies from the context and update context cookies from the response. The method will automatically follow redirects.

## api_request_context.put(url, **kwargs)<a name="api-request-context-put">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Target URL.<a name="api-request-context-put-option-url">#</a>
- `data` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)|[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)> Allows to set post data of the request. If the data parameter is an object, it will be serialized to json string and `content-type` header will be set to `application/json` if not explicitly set. Otherwise the `content-type` header will be set to `application/octet-stream` if not explicitly set.<a name="api-request-context-put-option-data">#</a>
- `fail_on_status_code` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to throw on response codes other than 2xx and 3xx. By default response object is returned for all status codes.<a name="api-request-context-put-option-fail-on-status-code">#</a>
- `form` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Provides an object that will be serialized as html form using `application/x-www-form-urlencoded` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `application/x-www-form-urlencoded` unless explicitly provided.<a name="api-request-context-put-option-form">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Allows to set HTTP headers.<a name="api-request-context-put-option-headers">#</a>
- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="api-request-context-put-option-ignore-https-errors">#</a>
- `multipart` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)|[ReadStream]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> Provides an object that will be serialized as html form using `multipart/form-data` encoding and sent as this request body. If this parameter is specified `content-type` header will be set to `multipart/form-data` unless explicitly provided. File values can be passed either as [`fs.ReadStream`](https://nodejs.org/api/fs.html#fs_class_fs_readstream) or as file-like object containing file name, mime-type and its content.<a name="api-request-context-put-option-multipart">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `params` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Query parameters to be sent with the URL.<a name="api-request-context-put-option-params">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Request timeout in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="api-request-context-put-option-timeout">#</a>
- returns: \<[APIResponse](#apiresponse)><a name="api-request-context-put-return">#</a>

## api_request_context.storage_state(**kwargs)<a name="api-request-context-storage-state">#</a>

- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> The file path to save the storage state to. If `path` is a relative path, then it is resolved relative to current working directory. If no path is provided, storage state is still returned, but won't be saved to the disk.<a name="api-request-context-storage-state-option-path">#</a>
- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="api-request-context-storage-state-return">#</a>
    - `cookies` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
        - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `domain` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `path` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `expires` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Unix time in seconds.
        - `httpOnly` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `secure` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `sameSite` \<"Strict"|"Lax"|"None">
    - `origins` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
        - `origin` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `localStorage` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
            - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
            - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

Returns storage state for this request context, contains current cookies and local storage snapshot if it was passed to the constructor.



# APIResponse

[APIResponse](#apiresponse) class represents responses returned by [api_request_context.get(url, **kwargs)](#api-request-context-get) and similar methods.

- Sync

```python
from playwright.sync_api import sync_playwright

with sync_playwright() as p:
    context = playwright.request.new_context()
    response = context.get("https://example.com/user/repos")
    assert response.ok
    assert response.status == 200
    assert response.headers["content-type"] == "application/json; charset=utf-8"
    assert response.json()["name"] == "foobar"
    assert response.body() == '{"status": "ok"}'
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright, Playwright

async def run(playwright: Playwright):
    context = await playwright.request.new_context()
    response = await context.get("https://example.com/user/repos")
    assert response.ok
    assert response.status == 200
    assert response.headers["content-type"] == "application/json; charset=utf-8"
    assert response.json()["name"] == "foobar"
    assert await response.body() == '{"status": "ok"}'


async def main():
    async with async_playwright() as playwright:
        await run(playwright)

asyncio.run(main())
```

## api_response.body()<a name="api-response-body">#</a>

- returns: \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)><a name="api-response-body-return">#</a>

Returns the buffer with response body.

## api_response.dispose()<a name="api-response-dispose">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="api-response-dispose-return">#</a>

Disposes the body of this response. If not called then the body will stay in memory until the context closes.

## api_response.headers<a name="api-response-headers">#</a>

- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="api-response-headers-return">#</a>

An object with all the response HTTP headers associated with this response.

## api_response.headers_array<a name="api-response-headers-array">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="api-response-headers-array-return">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the header.
    - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Value of the header.

An array with all the request HTTP headers associated with this response. Header names are not lower-cased. Headers with multiple entries, such as `Set-Cookie`, appear in the array multiple times.

## api_response.json()<a name="api-response-json">#</a>

- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="api-response-json-return">#</a>

Returns the JSON representation of response body.

This method will throw if the response body is not parsable via `JSON.parse`.

## api_response.ok<a name="api-response-ok">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="api-response-ok-return">#</a>

Contains a boolean stating whether the response was successful (status in the range 200-299) or not.

## api_response.status<a name="api-response-status">#</a>

- returns: \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="api-response-status-return">#</a>

Contains the status code of the response (e.g., 200 for a success).

## api_response.status_text<a name="api-response-status-text">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="api-response-status-text-return">#</a>

Contains the status text of the response (e.g. usually an "OK" for a success).

## api_response.text()<a name="api-response-text">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="api-response-text-return">#</a>

Returns the text representation of response body.

## api_response.url<a name="api-response-url">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="api-response-url-return">#</a>

Contains the URL of the response.



# Assertions

Playwright gives you Web-First Assertions with convenience methods for creating assertions that will wait and retry until the expected condition is met.

Consider the following example:

- Sync

```python
from playwright.sync_api import Page, expect

def test_status_becomes_submitted(page: Page) -> None:
    # ..
    page.click("#submit-button")
    expect(page.locator(".status")).to_have_text("Submitted")
```

- Async

```python
from playwright.async_api import Page, expect

async def test_status_becomes_submitted(page: Page) -> None:
    # ..
    await page.click("#submit-button")
    await expect(page.locator(".status")).to_have_text("Submitted")
```



Playwright will be re-testing the node with the selector `.status` until fetched Node has the `"Submitted"` text. It will be re-fetching the node and checking it over and over, until the condition is met or until the timeout is reached. You can pass this timeout as an option.

By default, the timeout for assertions is set to 5 seconds.

## expect(locator).not_to_be_checked(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-checked)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-checked-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-checked-return)

The opposite of [expect(locator).to_be_checked(**kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-checked).

## expect(locator).not_to_be_disabled(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-disabled)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-disabled-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-disabled-return)

The opposite of [expect(locator).to_be_disabled(**kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-disabled).

## expect(locator).not_to_be_editable(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-editable)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-editable-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-editable-return)

The opposite of [expect(locator).to_be_editable(**kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-editable).

## expect(locator).not_to_be_empty(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-empty)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-empty-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-empty-return)

The opposite of [expect(locator).to_be_empty(**kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-empty).

## expect(locator).not_to_be_enabled(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-enabled)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-enabled-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-enabled-return)

The opposite of [expect(locator).to_be_enabled(**kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-enabled).

## expect(locator).not_to_be_focused(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-focused)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-focused-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-focused-return)

The opposite of [expect(locator).to_be_focused(**kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-focused).

## expect(locator).not_to_be_hidden(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-hidden)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-hidden-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-hidden-return)

The opposite of [expect(locator).to_be_hidden(**kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-hidden).

## expect(locator).not_to_be_visible(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-visible)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-visible-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-be-visible-return)

The opposite of [expect(locator).to_be_visible(**kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-visible).

## expect(locator).not_to_contain_text(expected, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-contain-text)

- `expected` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)]> Expected substring or RegExp or a list of those.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-contain-text-option-expected)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-contain-text-option-timeout)
- `use_inner_text` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to use `element.innerText` instead of `element.textContent` when retrieving DOM node text.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-contain-text-option-use-inner-text)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-contain-text-return)

The opposite of [expect(locator).to_contain_text(expected, **kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-contain-text).

## expect(locator).not_to_have_attribute(name, value, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-attribute)

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Attribute name.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-attribute-option-name)
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Expected attribute value.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-attribute-option-value)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-attribute-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-attribute-return)

The opposite of [expect(locator).to_have_attribute(name, value, **kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-attribute).

## expect(locator).not_to_have_class(expected, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-class)

- `expected` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)]> Expected class or RegExp or a list of those.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-class-option-expected)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-class-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-class-return)

The opposite of [expect(locator).to_have_class(expected, **kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-class).

## expect(locator).not_to_have_count(count, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-count)

- `count` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Expected count.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-count-option-count)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-count-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-count-return)

The opposite of [expect(locator).to_have_count(count, **kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-count).

## expect(locator).not_to_have_css(name, value, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-css)

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> CSS property name.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-css-option-name)
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> CSS property value.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-css-option-value)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-css-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-css-return)

The opposite of [expect(locator).to_have_css(name, value, **kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-css).

## expect(locator).not_to_have_id(id, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-id)

- `id` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Element id.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-id-option-id)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-id-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-id-return)

The opposite of [expect(locator).to_have_id(id, **kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-id).

## expect(locator).not_to_have_js_property(name, value, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-js-property)

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Property name.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-js-property-option-name)
- `value` \<[Any](https://docs.python.org/3/library/typing.html#typing.Any)> Property value.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-js-property-option-value)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-js-property-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-js-property-return)

The opposite of [expect(locator).to_have_js_property(name, value, **kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-js-property).

## expect(locator).not_to_have_text(expected, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-text)

- `expected` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)]> Expected substring or RegExp or a list of those.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-text-option-expected)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-text-option-timeout)
- `use_inner_text` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to use `element.innerText` instead of `element.textContent` when retrieving DOM node text.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-text-option-use-inner-text)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-text-return)

The opposite of [expect(locator).to_have_text(expected, **kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-text).

## expect(locator).not_to_have_value(value, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-value)

- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Expected value.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-value-option-value)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-value-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-not-to-have-value-return)

The opposite of [expect(locator).to_have_value(value, **kwargs)](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-value).

## expect(locator).to_be_checked(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-checked)

- `checked` \<[bool](https://docs.python.org/3/library/stdtypes.html)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-checked-option-checked)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-checked-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-checked-return)

Ensures the [Locator](#locator) points to a checked input.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator(".subscribe")
expect(locator).to_be_checked()
```

- Async

```python
from playwright.async_api import expect

locator = page.locator(".subscribe")
await expect(locator).to_be_checked()
```



## expect(locator).to_be_disabled(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-disabled)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-disabled-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-disabled-return)

Ensures the [Locator](#locator) points to a disabled element.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("button.submit")
expect(locator).to_be_disabled()
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("button.submit")
await expect(locator).to_be_disabled()
```



## expect(locator).to_be_editable(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-editable)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-editable-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-editable-return)

Ensures the [Locator](#locator) points to an editable element.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator(".input")
expect(locator).to_be_editable()
```

- Async

```python
from playwright.async_api import expect

locator = page.locator(".input")
await expect(locator).to_be_editable()
```



## expect(locator).to_be_empty(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-empty)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-empty-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-empty-return)

Ensures the [Locator](#locator) points to an empty editable element or to a DOM node that has no text.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("div.warning")
expect(locator).to_be_empty()
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("div.warning")
await expect(locator).to_be_empty()
```



## expect(locator).to_be_enabled(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-enabled)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-enabled-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-enabled-return)

Ensures the [Locator](#locator) points to an enabled element.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("button.submit")
expect(locator).to_be_enabled()
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("button.submit")
await expect(locator).to_be_enabled()
```



## expect(locator).to_be_focused(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-focused)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-focused-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-focused-return)

Ensures the [Locator](#locator) points to a focused DOM node.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator('input')
expect(locator).to_be_focused()
```

- Async

```python
from playwright.async_api import expect

locator = page.locator('input')
await expect(locator).to_be_focused()
```



## expect(locator).to_be_hidden(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-hidden)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-hidden-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-hidden-return)

Ensures the [Locator](#locator) points to a hidden DOM node, which is the opposite of [visible](https://playwright.dev/python/docs/actionability#visible).

- Sync

```python
from playwright.sync_api import expect

locator = page.locator('.my-element')
expect(locator).to_be_hidden()
```

- Async

```python
from playwright.async_api import expect

locator = page.locator('.my-element')
await expect(locator).to_be_hidden()
```



## expect(locator).to_be_visible(**kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-visible)

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-visible-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-be-visible-return)

Ensures the [Locator](#locator) points to a [visible](https://playwright.dev/python/docs/actionability#visible) DOM node.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator('.my-element')
expect(locator).to_be_visible()
```

- Async

```python
from playwright.async_api import expect

locator = page.locator('.my-element')
await expect(locator).to_be_visible()
```



## expect(locator).to_contain_text(expected, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-contain-text)

- `expected` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)]> Expected substring or RegExp or a list of those.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-contain-text-option-expected)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-contain-text-option-timeout)
- `use_inner_text` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to use `element.innerText` instead of `element.textContent` when retrieving DOM node text.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-contain-text-option-use-inner-text)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-contain-text-return)

Ensures the [Locator](#locator) points to an element that contains the given text. You can use regular expressions for the value as well.

- Sync

```python
import re
from playwright.sync_api import expect

locator = page.locator('.title')
expect(locator).to_contain_text("substring")
expect(locator).to_contain_text(re.compile(r"\d messages"))
```

- Async

```python
import re
from playwright.async_api import expect

locator = page.locator('.title')
await expect(locator).to_contain_text("substring")
await expect(locator).to_contain_text(re.compile(r"\d messages"))
```



Note that if array is passed as an expected value, entire lists of elements can be asserted:

- Sync

```python
import re
from playwright.sync_api import expect

locator = page.locator("list > .list-item")
expect(locator).to_contain_text(["Text 1", "Text 4", "Text 5"])
```

- Async

```python
import re
from playwright.async_api import expect

locator = page.locator("list > .list-item")
await expect(locator).to_contain_text(["Text 1", "Text 4", "Text 5"])
```



## expect(locator).to_have_attribute(name, value, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-attribute)

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Attribute name.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-attribute-option-name)
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Expected attribute value.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-attribute-option-value)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-attribute-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-attribute-return)

Ensures the [Locator](#locator) points to an element with given attribute.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("input")
expect(locator).to_have_attribute("type", "text")
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("input")
await expect(locator).to_have_attribute("type", "text")
```



## expect(locator).to_have_class(expected, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-class)

- `expected` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)]> Expected class or RegExp or a list of those.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-class-option-expected)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-class-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-class-return)

Ensures the [Locator](#locator) points to an element with given CSS class.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("#component")
expect(locator).to_have_class(re.compile(r"selected"))
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("#component")
await expect(locator).to_have_class(re.compile(r"selected"))
```



Note that if array is passed as an expected value, entire lists of elements can be asserted:

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("list > .component")
expect(locator).to_have_class(["component", "component selected", "component"])
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("list > .component")
await expect(locator).to_have_class(["component", "component selected", "component"])
```



## expect(locator).to_have_count(count, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-count)

- `count` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Expected count.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-count-option-count)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-count-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-count-return)

Ensures the [Locator](#locator) resolves to an exact number of DOM nodes.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("list > .component")
expect(locator).to_have_count(3)
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("list > .component")
await expect(locator).to_have_count(3)
```



## expect(locator).to_have_css(name, value, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-css)

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> CSS property name.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-css-option-name)
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> CSS property value.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-css-option-value)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-css-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-css-return)

Ensures the [Locator](#locator) resolves to an element with the given computed CSS style.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("button")
expect(locator).to_have_css("display", "flex")
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("button")
await expect(locator).to_have_css("display", "flex")
```



## expect(locator).to_have_id(id, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-id)

- `id` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Element id.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-id-option-id)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-id-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-id-return)

Ensures the [Locator](#locator) points to an element with the given DOM Node ID.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("input")
expect(locator).to_have_id("lastname")
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("input")
await expect(locator).to_have_id("lastname")
```



## expect(locator).to_have_js_property(name, value, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-js-property)

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Property name.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-js-property-option-name)
- `value` \<[Any](https://docs.python.org/3/library/typing.html#typing.Any)> Property value.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-js-property-option-value)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-js-property-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-js-property-return)

Ensures the [Locator](#locator) points to an element with given JavaScript property. Note that this property can be of a primitive type as well as a plain serializable JavaScript object.

- Sync

```python
from playwright.sync_api import expect

locator = page.locator(".component")
expect(locator).to_have_js_property("loaded", True)
```

- Async

```python
from playwright.async_api import expect

locator = page.locator(".component")
await expect(locator).to_have_js_property("loaded", True)
```



## expect(locator).to_have_text(expected, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-text)

- `expected` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)]> Expected substring or RegExp or a list of those.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-text-option-expected)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-text-option-timeout)
- `use_inner_text` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to use `element.innerText` instead of `element.textContent` when retrieving DOM node text.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-text-option-use-inner-text)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-text-return)

Ensures the [Locator](#locator) points to an element with the given text. You can use regular expressions for the value as well.

- Sync

```python
import re
from playwright.sync_api import expect

locator = page.locator(".title")
expect(locator).to_have_text(re.compile(r"Welcome, Test User"))
expect(locator).to_have_text(re.compile(r"Welcome, .*"))
```

- Async

```python
import re
from playwright.async_api import expect

locator = page.locator(".title")
await expect(locator).to_have_text(re.compile(r"Welcome, Test User"))
await expect(locator).to_have_text(re.compile(r"Welcome, .*"))
```



Note that if array is passed as an expected value, entire lists of elements can be asserted:

- Sync

```python
from playwright.sync_api import expect

locator = page.locator("list > .component")
expect(locator).to_have_text(["Text 1", "Text 2", "Text 3"])
```

- Async

```python
from playwright.async_api import expect

locator = page.locator("list > .component")
await expect(locator).to_have_text(["Text 1", "Text 2", "Text 3"])
```



## expect(locator).to_have_value(value, **kwargs)[](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-value)

- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Expected value.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-value-option-value)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-value-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#locator-assertions-to-have-value-return)

Ensures the [Locator](#locator) points to an element with the given input value. You can use regular expressions for the value as well.

- Sync

```python
import re
from playwright.sync_api import expect

locator = page.locator("input[type=number]")
expect(locator).to_have_value(re.compile(r"[0-9]"))
```

- Async

```python
import re
from playwright.async_api import expect

locator = page.locator("input[type=number]")
await expect(locator).to_have_value(re.compile(r"[0-9]"))
```



## expect(page).not_to_have_title(title_or_reg_exp, **kwargs)[](https://playwright.dev/python/docs/test-assertions#page-assertions-not-to-have-title)

- `title_or_reg_exp` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Expected title or RegExp.[#](https://playwright.dev/python/docs/test-assertions#page-assertions-not-to-have-title-option-title-or-reg-exp)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#page-assertions-not-to-have-title-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#page-assertions-not-to-have-title-return)

The opposite of [expect(page).to_have_title(title_or_reg_exp, **kwargs)](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-title).

## expect(page).not_to_have_url(url_or_reg_exp, **kwargs)[](https://playwright.dev/python/docs/test-assertions#page-assertions-not-to-have-url)

- `url_or_reg_exp` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Expected substring or RegExp.[#](https://playwright.dev/python/docs/test-assertions#page-assertions-not-to-have-url-option-url-or-reg-exp)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#page-assertions-not-to-have-url-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#page-assertions-not-to-have-url-return)

The opposite of [expect(page).to_have_url(url_or_reg_exp, **kwargs)](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-url).

## expect(page).to_have_title(title_or_reg_exp, **kwargs)[](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-title)

- `title_or_reg_exp` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Expected title or RegExp.[#](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-title-option-title-or-reg-exp)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-title-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-title-return)

Ensures the page has the given title.

- Sync

```python
import re
from playwright.sync_api import expect

# ...
expect(page).to_have_title(re.compile(r".*checkout"))
```

- Async

```python
import re
from playwright.async_api import expect

# ...
await expect(page).to_have_title(re.compile(r".*checkout"))
```



## expect(page).to_have_url(url_or_reg_exp, **kwargs)[](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-url)

- `url_or_reg_exp` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Expected substring or RegExp.[#](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-url-option-url-or-reg-exp)
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to retry the assertion for.[#](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-url-option-timeout)
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#page-assertions-to-have-url-return)

Ensures the page is navigated to the given URL.

- Sync

```python
import re
from playwright.sync_api import expect

# ...
expect(page).to_have_url(re.compile(".*checkout"))
```

- Async

```python
import re
from playwright.async_api import expect

# ...
await expect(page).to_have_url(re.compile(".*checkout"))
```



## expect(api_response).not_to_be_ok()[](https://playwright.dev/python/docs/test-assertions#api-response-assertions-not-to-be-ok)

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#api-response-assertions-not-to-be-ok-return)

The opposite of [expect(api_response).to_be_ok()](https://playwright.dev/python/docs/test-assertions#api-response-assertions-to-be-ok).

## expect(api_response).to_be_ok()[](https://playwright.dev/python/docs/test-assertions#api-response-assertions-to-be-ok)

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)>[#](https://playwright.dev/python/docs/test-assertions#api-response-assertions-to-be-ok-return)

Ensures the response status code is within [200..299] range.

- Sync

```python
import re
from playwright.sync_api import expect

# ...
expect(response).to_be_ok()
```

- Async

```python
from playwright.async_api import expect

# ...
await expect(response).to_be_ok()
```





# Accessibility

The Accessibility class provides methods for inspecting Chromium's accessibility tree. The accessibility tree is used by assistive technology such as [screen readers](https://en.wikipedia.org/wiki/Screen_reader) or [switches](https://en.wikipedia.org/wiki/Switch_access).

Accessibility is a very platform-specific thing. On different platforms, there are different screen readers that might have wildly different output.

Rendering engines of Chromium, Firefox and WebKit have a concept of "accessibility tree", which is then translated into different platform-specific APIs. Accessibility namespace gives access to this Accessibility Tree.

Most of the accessibility tree gets filtered out when converting from internal browser AX Tree to Platform-specific AX-Tree or by assistive technologies themselves. By default, Playwright tries to approximate this filtering, exposing only the "interesting" nodes of the tree.

- [accessibility.snapshot(**kwargs)](#accessibility-snapshot)

## accessibility.snapshot(**kwargs)<a name="accessibility-snapshot">#</a>

- `interesting_only` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Prune uninteresting nodes from the tree. Defaults to `true`.<a name="accessibility-snapshot-option-interesting-only">#</a>
- `root` \<[ElementHandle](#elementhandle)> The root DOM element for the snapshot. Defaults to the whole page.<a name="accessibility-snapshot-option-root">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="accessibility-snapshot-return">#</a>
    - `role` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> The [role](https://www.w3.org/TR/wai-aria/#usage_intro).
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A human readable name for the node.
    - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> The current value of the node, if applicable.
    - `description` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> An additional human readable description of the node, if applicable.
    - `keyshortcuts` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Keyboard shortcuts associated with this node, if applicable.
    - `roledescription` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A human readable alternative to the role, if applicable.
    - `valuetext` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A description of the current value, if applicable.
    - `disabled` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the node is disabled, if applicable.
    - `expanded` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the node is expanded or collapsed, if applicable.
    - `focused` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the node is focused, if applicable.
    - `modal` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the node is [modal](https://en.wikipedia.org/wiki/Modal_window), if applicable.
    - `multiline` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the node text input supports multiline, if applicable.
    - `multiselectable` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether more than one child can be selected, if applicable.
    - `readonly` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the node is read only, if applicable.
    - `required` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the node is required, if applicable.
    - `selected` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the node is selected in its parent node, if applicable.
    - `checked` \<[bool](https://docs.python.org/3/library/stdtypes.html)|"mixed"> Whether the checkbox is checked, or "mixed", if applicable.
    - `pressed` \<[bool](https://docs.python.org/3/library/stdtypes.html)|"mixed"> Whether the toggle button is checked, or "mixed", if applicable.
    - `level` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> The level of a heading, if applicable.
    - `valuemin` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> The minimum value in a node, if applicable.
    - `valuemax` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> The maximum value in a node, if applicable.
    - `autocomplete` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> What kind of autocomplete is supported by a control, if applicable.
    - `haspopup` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> What kind of popup is currently being shown for a node, if applicable.
    - `invalid` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Whether and in what way this node's value is invalid, if applicable.
    - `orientation` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Whether the node is oriented horizontally or vertically, if applicable.
    - `children` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> Child nodes, if any, if applicable.

Captures the current state of the accessibility tree. The returned object represents the root accessible node of the page.





> NOTE
>
> The Chromium accessibility tree contains nodes that go unused on most platforms and by most screen readers. Playwright will discard them as well for an easier to process tree, unless `interesting_only` is set to `false`.
>
> 

An example of dumping the entire accessibility tree:

- Sync

```python
snapshot = page.accessibility.snapshot()
print(snapshot)
```

- Async

```python
snapshot = await page.accessibility.snapshot()
print(snapshot)
```

An example of logging the focused node's name:

- Sync

```python
def find_focused_node(node):
    if (node.get("focused"))
        return node
    for child in (node.get("children") or []):
        found_node = find_focused_node(child)
        return found_node
    return None

snapshot = page.accessibility.snapshot()
node = find_focused_node(snapshot)
if node:
    print(node["name"])
```

- Async

```python
def find_focused_node(node):
    if (node.get("focused"))
        return node
    for child in (node.get("children") or []):
        found_node = find_focused_node(child)
        return found_node
    return None

snapshot = await page.accessibility.snapshot()
node = find_focused_node(snapshot)
if node:
    print(node["name"])
```



# Browser

- extends: [EventEmitter](https://pyee.readthedocs.io/en/latest/#pyee.BaseEventEmitter)

A Browser is created via [browser_type.launch(**kwargs)](#browser-type-launch). An example of using a [Browser](#browser) to create a [Page](#page):

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    firefox = playwright.firefox
    browser = firefox.launch()
    page = browser.new_page()
    page.goto("https://example.com")
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    firefox = playwright.firefox
    browser = await firefox.launch()
    page = await browser.new_page()
    await page.goto("https://example.com")
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

- [browser.on("disconnected")](#browser-event-disconnected)
- [browser.close()](#browser-close)
- [browser.contexts](#browser-contexts)
- [browser.is_connected()](#browser-is-connected)
- [browser.new_browser_cdp_session()](#browser-new-browser-cdp-session)
- [browser.new_context(**kwargs)](#browser-new-context)
- [browser.new_page(**kwargs)](#browser-new-page)
- [browser.start_tracing(**kwargs)](#browser-start-tracing)
- [browser.stop_tracing()](#browser-stop-tracing)
- [browser.version](#browser-version)

## browser.on("disconnected")<a name="browser-event-disconnected">#</a>

- type: \<[Browser](#browser)>

Emitted when Browser gets disconnected from the browser application. This might happen because of one of the following:

- Browser application is closed or crashed.
- The [browser.close()](#browser-close) method was called.

## browser.close()<a name="browser-close">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-close-return">#</a>

In case this browser is obtained using [browser_type.launch(**kwargs)](#browser-type-launch), closes the browser and all of its pages (if any were opened).

In case this browser is connected to, clears all created contexts belonging to this browser and disconnects from the browser server.

The [Browser](#browser) object itself is considered to be disposed and cannot be used anymore.

## browser.contexts<a name="browser-contexts">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[BrowserContext](#browsercontext)]><a name="browser-contexts-return">#</a>

Returns an array of all open browser contexts. In a newly created browser, this will return zero browser contexts.

- Sync

```python
browser = pw.webkit.launch()
print(len(browser.contexts())) # prints `0`
context = browser.new_context()
print(len(browser.contexts())) # prints `1`
```

- Async

```python
browser = await pw.webkit.launch()
print(len(browser.contexts())) # prints `0`
context = await browser.new_context()
print(len(browser.contexts())) # prints `1`
```

## browser.is_connected()<a name="browser-is-connected">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="browser-is-connected-return">#</a>

Indicates that the browser is connected.

## browser.new_browser_cdp_session()<a name="browser-new-browser-cdp-session">#</a>

- returns: \<[CDPSession](#cdpsession)><a name="browser-new-browser-cdp-session-return">#</a>

> NOTE
>
> CDP Sessions are only supported on Chromium-based browsers.
>
> Returns the newly created browser session.

## browser.new_context(**kwargs)<a name="browser-new-context">#</a>

- `accept_downloads` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to automatically download all the attachments. Defaults to `true` where all the downloads are accepted.<a name="browser-new-context-option-accept-downloads">#</a>
- `base_url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> When using [page.goto(url, **kwargs)](#page-goto), [page.route(url, handler, **kwargs)](#page-route), [page.wait_for_url(url, **kwargs)](#page-wait-for-url), [page.expect_request(url_or_predicate, **kwargs)](#page-wait-for-request), or [page.expect_response(url_or_predicate, **kwargs)](#page-wait-for-response) it takes the base URL in consideration by using the [`URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) constructor for building the corresponding URL. Examples:
    - baseURL: `http://localhost:3000` and navigating to `/bar.html` results in `http://localhost:3000/bar.html`
    - baseURL: `http://localhost:3000/foo/` and navigating to `./bar.html` results in `http://localhost:3000/foo/bar.html`
    - baseURL: `http://localhost:3000/foo` (without trailing slash) and navigating to `./bar.html` results in `http://localhost:3000/bar.html`
- `bypass_csp` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Toggles bypassing page's Content-Security-Policy.<a name="browser-new-context-option-bypass-csp">#</a>
- `color_scheme` \<"light"|"dark"|"no-preference"> Emulates `'prefers-colors-scheme'` media feature, supported values are `'light'`, `'dark'`, `'no-preference'`. See [page.emulate_media(**kwargs)](#page-emulate-media) for more details. Defaults to `'light'`.<a name="browser-new-context-option-color-scheme">#</a>
- `device_scale_factor` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Specify device scale factor (can be thought of as dpr). Defaults to `1`.<a name="browser-new-context-option-device-scale-factor">#</a>
- `extra_http_headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> An object containing additional HTTP headers to be sent with every request.<a name="browser-new-context-option-extra-http-headers">#</a>
- `forced_colors` \<"active"|"none"> Emulates `'forced-colors'` media feature, supported values are `'active'`, `'none'`. See [page.emulate_media(**kwargs)](#page-emulate-media) for more details. Defaults to `'none'`.<a name="browser-new-context-option-forced-colors">#</a>

> NOTE
>
> It's not supported in WebKit, see [here](https://bugs.webkit.org/show_bug.cgi?id=225281) in their issue tracker.

- `geolocation` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="browser-new-context-option-geolocation">#</a>

    - `latitude` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Latitude between -90 and 90.
    - `longitude` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Longitude between -180 and 180.
    - `accuracy` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Non-negative accuracy value. Defaults to `0`.

- `has_touch` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Specifies if viewport supports touch events. Defaults to false.<a name="browser-new-context-option-has-touch">#</a>

- `http_credentials` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Credentials for [HTTP authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication).<a name="browser-new-context-option-http-credentials">#</a>

    - `username` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `password` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="browser-new-context-option-ignore-https-errors">#</a>

- `is_mobile` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the `meta viewport` tag is taken into account and touch events are enabled. Defaults to `false`. Not supported in Firefox.<a name="browser-new-context-option-is-mobile">#</a>

- `java_script_enabled` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether or not to enable JavaScript in the context. Defaults to `true`.<a name="browser-new-context-option-java-script-enabled">#</a>

- `locale` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Specify user locale, for example `en-GB`, `de-DE`, etc. Locale will affect `navigator.language` value, `Accept-Language` request header value as well as number and date formatting rules.<a name="browser-new-context-option-locale">#</a>

- `no_viewport` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Does not enforce fixed viewport, allows resizing window in the headed mode.<a name="browser-new-context-option-no-viewport">#</a>

- `offline` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to emulate network being offline. Defaults to `false`.<a name="browser-new-context-option-offline">#</a>

- `permissions` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> A list of permissions to grant to all pages in this context. See [browser_context.grant_permissions(permissions, **kwargs)](#browser-context-grant-permissions) for more details.<a name="browser-new-context-option-permissions">#</a>

- `proxy` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Network proxy settings to use with this context.<a name="browser-new-context-option-proxy">#</a>

    - `server` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Proxy to be used for all requests. HTTP and SOCKS proxies are supported, for example `http://myproxy.com:3128` or `socks5://myproxy.com:3128`. Short form `myproxy.com:3128` is considered an HTTP proxy.
    - `bypass` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional comma-separated domains to bypass proxy, for example `".com, chromium.org, .domain.com"`.
    - `username` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional username to use if HTTP proxy requires authentication.
    - `password` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional password to use if HTTP proxy requires authentication.

    > NOTE
    >
    > For Chromium on Windows the browser needs to be launched with the global proxy for this option to work. If all contexts override the proxy, global proxy will be never used and can be any string, for example `launch({ proxy: { server: 'http://per-context' } })`.

- `record_har_omit_content` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Optional setting to control whether to omit request content from the HAR. Defaults to `false`.<a name="browser-new-context-option-record-har-omit-content">#</a>

- `record_har_path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Enables [HAR](http://www.softwareishard.com/blog/har-12-spec) recording for all pages into the specified HAR file on the filesystem. If not specified, the HAR is not recorded. Make sure to call [browser_context.close()](#browser-context-close) for the HAR to be saved.<a name="browser-new-context-option-record-har-path">#</a>

- `record_video_dir` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Enables video recording for all pages into the specified directory. If not specified videos are not recorded. Make sure to call [browser_context.close()](#browser-context-close) for videos to be saved.<a name="browser-new-context-option-record-video-dir">#</a>

- `record_video_size` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Dimensions of the recorded videos. If not specified the size will be equal to `viewport` scaled down to fit into 800x800. If `viewport` is not configured explicitly the video size defaults to 800x450. Actual picture of each page will be scaled down if necessary to fit the specified size.<a name="browser-new-context-option-record-video-size">#</a>

    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Video frame width.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Video frame height.

- `reduced_motion` \<"reduce"|"no-preference"> Emulates `'prefers-reduced-motion'` media feature, supported values are `'reduce'`, `'no-preference'`. See [page.emulate_media(**kwargs)](#page-emulate-media) for more details. Defaults to `'no-preference'`.<a name="browser-new-context-option-reduced-motion">#</a>

- `screen` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Emulates consistent window screen size available inside web page via `window.screen`. Is only used when the `viewport` is set.<a name="browser-new-context-option-screen">#</a>

    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page width in pixels.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page height in pixels.

- `storage_state` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Populates context with given storage state. This option can be used to initialize context with logged-in information obtained via [browser_context.storage_state(**kwargs)](#browser-context-storage-state). Either a path to the file with saved storage, or an object with the following fields:<a name="browser-new-context-option-storage-state">#</a>

    - `cookies` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> cookies to set for context
        - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `domain` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> domain and path are required
        - `path` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> domain and path are required
        - `expires` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Unix time in seconds.
        - `httpOnly` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `secure` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `sameSite` \<"Strict"|"Lax"|"None"> sameSite flag
    - `origins` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> localStorage to set for context
        - `origin` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `localStorage` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
            - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
            - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

- `strict_selectors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> It specified, enables strict selectors mode for this context. In the strict selectors mode all operations on selectors that imply single target DOM element will throw when more than one element matches the selector. See [Locator](#locator) to learn more about the strict mode.<a name="browser-new-context-option-strict-selectors">#</a>

- `timezone_id` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Changes the timezone of the context. See [ICU's metaZones.txt](https://cs.chromium.org/chromium/src/third_party/icu/source/data/misc/metaZones.txt?rcl=faee8bc70570192d82d2978a71e2a615788597d1) for a list of supported timezone IDs.<a name="browser-new-context-option-timezone-id">#</a>

- `user_agent` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Specific user agent to use in this context.<a name="browser-new-context-option-user-agent">#</a>

- `viewport` \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Sets a consistent viewport for each page. Defaults to an 1280x720 viewport. `no_viewport` disables the fixed viewport.<a name="browser-new-context-option-viewport">#</a>

    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page width in pixels.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page height in pixels.

- returns: \<[BrowserContext](#browsercontext)><a name="browser-new-context-return">#</a>

Creates a new browser context. It won't share cookies/cache with other browser contexts.

- Sync

```python
browser = playwright.firefox.launch() # or "chromium" or "webkit".
# create a new incognito browser context.
context = browser.new_context()
# create a new page in a pristine context.
page = context.new_page()
page.goto("https://example.com")
```

- Async

```python
browser = await playwright.firefox.launch() # or "chromium" or "webkit".
# create a new incognito browser context.
context = await browser.new_context()
# create a new page in a pristine context.
page = await context.new_page()
await page.goto("https://example.com")
```

## browser.new_page(**kwargs)<a name="browser-new-page">#</a>

- `accept_downloads` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to automatically download all the attachments. Defaults to `true` where all the downloads are accepted.<a name="browser-new-page-option-accept-downloads">#</a>

- `base_url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> When using [page.goto(url, **kwargs)](#page-goto), [page.route(url, handler, **kwargs)](#page-route), [page.wait_for_url(url, **kwargs)](#page-wait-for-url), [page.expect_request(url_or_predicate, **kwargs)](#page-wait-for-request), or [page.expect_response(url_or_predicate, **kwargs)](#page-wait-for-response) it takes the base URL in consideration by using the [`URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) constructor for building the corresponding URL. Examples:<a name="browser-new-page-option-base-url">#</a>

    - baseURL: `http://localhost:3000` and navigating to `/bar.html` results in `http://localhost:3000/bar.html`
    - baseURL: `http://localhost:3000/foo/` and navigating to `./bar.html` results in `http://localhost:3000/foo/bar.html`
    - baseURL: `http://localhost:3000/foo` (without trailing slash) and navigating to `./bar.html` results in `http://localhost:3000/bar.html`

- `bypass_csp` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Toggles bypassing page's Content-Security-Policy.<a name="browser-new-page-option-bypass-csp">#</a>

- `color_scheme` \<"light"|"dark"|"no-preference"> Emulates `'prefers-colors-scheme'` media feature, supported values are `'light'`, `'dark'`, `'no-preference'`. See [page.emulate_media(**kwargs)](#page-emulate-media) for more details. Defaults to `'light'`.<a name="browser-new-page-option-color-scheme">#</a>

- `device_scale_factor` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Specify device scale factor (can be thought of as dpr). Defaults to `1`.<a name="browser-new-page-option-device-scale-factor">#</a>

- `extra_http_headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> An object containing additional HTTP headers to be sent with every request.<a name="browser-new-page-option-extra-http-headers">#</a>

- `forced_colors` \<"active"|"none"> Emulates `'forced-colors'` media feature, supported values are `'active'`, `'none'`. See [page.emulate_media(**kwargs)](#page-emulate-media) for more details. Defaults to `'none'`.<a name="browser-new-page-option-forced-colors">#</a>

    > NOTE
    >
    > It's not supported in WebKit, see [here](https://bugs.webkit.org/show_bug.cgi?id=225281) in their issue tracker.

- `geolocation` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="browser-new-page-option-geolocation">#</a>

    - `latitude` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Latitude between -90 and 90.
    - `longitude` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Longitude between -180 and 180.
    - `accuracy` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Non-negative accuracy value. Defaults to `0`.

- `has_touch` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Specifies if viewport supports touch events. Defaults to false.<a name="browser-new-page-option-has-touch">#</a>

- `http_credentials` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Credentials for [HTTP authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication).<a name="browser-new-page-option-http-credentials">#</a>

    - `username` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `password` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="browser-new-page-option-ignore-https-errors">#</a>

- `is_mobile` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the `meta viewport` tag is taken into account and touch events are enabled. Defaults to `false`. Not supported in Firefox.<a name="browser-new-page-option-is-mobile">#</a>

- `java_script_enabled` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether or not to enable JavaScript in the context. Defaults to `true`.<a name="browser-new-page-option-java-script-enabled">#</a>

- `locale` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Specify user locale, for example `en-GB`, `de-DE`, etc. Locale will affect `navigator.language` value, `Accept-Language` request header value as well as number and date formatting rules.<a name="browser-new-page-option-locale">#</a>

- `no_viewport` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Does not enforce fixed viewport, allows resizing window in the headed mode.<a name="browser-new-page-option-no-viewport">#</a>

- `offline` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to emulate network being offline. Defaults to `false`.<a name="browser-new-page-option-offline">#</a>

- `permissions` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> A list of permissions to grant to all pages in this context. See [browser_context.grant_permissions(permissions, **kwargs)](#browser-context-grant-permissions) for more details.<a name="browser-new-page-option-permissions">#</a>

- `proxy` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Network proxy settings to use with this context.<a name="browser-new-page-option-proxy">#</a>

    - `server` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Proxy to be used for all requests. HTTP and SOCKS proxies are supported, for example `http://myproxy.com:3128` or `socks5://myproxy.com:3128`. Short form `myproxy.com:3128` is considered an HTTP proxy.
    - `bypass` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional comma-separated domains to bypass proxy, for example `".com, chromium.org, .domain.com"`.
    - `username` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional username to use if HTTP proxy requires authentication.
    - `password` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional password to use if HTTP proxy requires authentication.

    > NOTE
    >
    > For Chromium on Windows the browser needs to be launched with the global proxy for this option to work. If all contexts override the proxy, global proxy will be never used and can be any string, for example `launch({ proxy: { server: 'http://per-context' } })`.

- `record_har_omit_content` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Optional setting to control whether to omit request content from the HAR. Defaults to `false`.<a name="browser-new-page-option-record-har-omit-content">#</a>

- `record_har_path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Enables [HAR](http://www.softwareishard.com/blog/har-12-spec) recording for all pages into the specified HAR file on the filesystem. If not specified, the HAR is not recorded. Make sure to call [browser_context.close()](#browser-context-close) for the HAR to be saved.<a name="browser-new-page-option-record-har-path">#</a>

- `record_video_dir` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Enables video recording for all pages into the specified directory. If not specified videos are not recorded. Make sure to call [browser_context.close()](#browser-context-close) for videos to be saved.<a name="browser-new-page-option-record-video-dir">#</a>

- `record_video_size` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Dimensions of the recorded videos. If not specified the size will be equal to `viewport` scaled down to fit into 800x800. If `viewport` is not configured explicitly the video size defaults to 800x450. Actual picture of each page will be scaled down if necessary to fit the specified size.<a name="browser-new-page-option-record-video-size">#</a>

    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Video frame width.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Video frame height.

- `reduced_motion` \<"reduce"|"no-preference"> Emulates `'prefers-reduced-motion'` media feature, supported values are `'reduce'`, `'no-preference'`. See [page.emulate_media(**kwargs)](#page-emulate-media) for more details. Defaults to `'no-preference'`.<a name="browser-new-page-option-reduced-motion">#</a>

- `screen` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Emulates consistent window screen size available inside web page via `window.screen`. Is only used when the `viewport` is set.<a name="browser-new-page-option-screen">#</a>

    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page width in pixels.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page height in pixels.

- `storage_state` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Populates context with given storage state. This option can be used to initialize context with logged-in information obtained via [browser_context.storage_state(**kwargs)](#browser-context-storage-state). Either a path to the file with saved storage, or an object with the following fields:<a name="browser-new-page-option-storage-state">#</a>

    - `cookies` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> cookies to set for context
        - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `domain` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> domain and path are required
        - `path` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> domain and path are required
        - `expires` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Unix time in seconds.
        - `httpOnly` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `secure` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `sameSite` \<"Strict"|"Lax"|"None"> sameSite flag
    - `origins` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]> localStorage to set for context
        - `origin` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `localStorage` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
            - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
            - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

- `strict_selectors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> It specified, enables strict selectors mode for this context. In the strict selectors mode all operations on selectors that imply single target DOM element will throw when more than one element matches the selector. See [Locator](#locator) to learn more about the strict mode.<a name="browser-new-page-option-strict-selectors">#</a>

- `timezone_id` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Changes the timezone of the context. See [ICU's metaZones.txt](https://cs.chromium.org/chromium/src/third_party/icu/source/data/misc/metaZones.txt?rcl=faee8bc70570192d82d2978a71e2a615788597d1) for a list of supported timezone IDs.<a name="browser-new-page-option-timezone-id">#</a>

- `user_agent` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Specific user agent to use in this context.<a name="browser-new-page-option-user-agent">#</a>

- `viewport` \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Sets a consistent viewport for each page. Defaults to an 1280x720 viewport. `no_viewport` disables the fixed viewport.

    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page width in pixels.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page height in pixels.

- returns: \<[Page](#page)><a name="browser-new-page-return">#</a>

Creates a new page in a new browser context. Closing this page will close the context as well.

This is a convenience API that should only be used for the single-page scenarios and short snippets. Production code and testing frameworks should explicitly create [browser.new_context(**kwargs)](#browser-new-context) followed by the [browser_context.new_page()](#browser-context-new-page) to control their exact life times.

## browser.start_tracing(**kwargs)<a name="browser-start-tracing">#</a>

- `page` \<[Page](#page)> Optional, if specified, tracing includes screenshots of the given page.<a name="browser-start-tracing-option-page">#</a>
- `categories` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> specify custom categories to use instead of default.<a name="browser-start-tracing-option-categories">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> A path to write the trace file to.<a name="browser-start-tracing-option-path">#</a>
- `screenshots` \<[bool](https://docs.python.org/3/library/stdtypes.html)> captures screenshots in the trace.<a name="browser-start-tracing-option-screenshots">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-start-tracing-return">#</a>

> NOTE
>
> This API controls [Chromium Tracing](https://www.chromium.org/developers/how-tos/trace-event-profiling-tool) which is a low-level chromium-specific debugging tool. API to control [Playwright Tracing](https://playwright.dev/python/docs/trace-viewer) could be found [here](#tracing).
>
> You can use [browser.start_tracing(**kwargs)](#browser-start-tracing) and [browser.stop_tracing()](#browser-stop-tracing) to create a trace file that can be opened in Chrome DevTools performance panel.

- Sync

```python
browser.start_tracing(page, path="trace.json")
page.goto("https://www.google.com")
browser.stop_tracing()
```

- Async

```python
await browser.start_tracing(page, path="trace.json")
await page.goto("https://www.google.com")
await browser.stop_tracing()
```

## browser.stop_tracing()<a name="browser-stop-tracing">#</a>

- returns: \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)><a name="browser-stop-tracing-return">#</a>

> NOTE
>
> This API controls [Chromium Tracing](https://www.chromium.org/developers/how-tos/trace-event-profiling-tool) which is a low-level chromium-specific debugging tool. API to control [Playwright Tracing](https://playwright.dev/python/docs/trace-viewer) could be found [here](#tracing).
>
> Returns the buffer with trace data.

## browser.version<a name="browser-version">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="browser-version-return">#</a>

Returns the browser version.



# BrowserContext

- extends: [EventEmitter](https://pyee.readthedocs.io/en/latest/#pyee.BaseEventEmitter)

BrowserContexts provide a way to operate multiple independent browser sessions.

If a page opens another page, e.g. with a `window.open` call, the popup will belong to the parent page's browser context.

Playwright allows creating "incognito" browser contexts with [browser.new_context(**kwargs)](#browser-new-context) method. "Incognito" browser contexts don't write any browsing data to disk.

- Sync

```python
# create a new incognito browser context
context = browser.new_context()
# create a new page inside context.
page = context.new_page()
page.goto("https://example.com")
# dispose context once it is no longer needed.
context.close()
```

- Async

```python
# create a new incognito browser context
context = await browser.new_context()
# create a new page inside context.
page = await context.new_page()
await page.goto("https://example.com")
# dispose context once it is no longer needed.
await context.close()
```

## browser_context.on("backgroundpage")<a name="browser-context-event-background-page">#</a>

- type: \<[Page](#page)>

> NOTE
>
> Only works with Chromium browser's persistent context.
>
> Emitted when new background page is created in the context.

- Sync

```python
background_page = context.wait_for_event("backgroundpage")
```

- Async

```python
background_page = await context.wait_for_event("backgroundpage")
```



## browser_context.on("close")<a name="browser-context-event-close">#</a>

- type: \<[BrowserContext](#browsercontext)>

Emitted when Browser context gets closed. This might happen because of one of the following:

- Browser context is closed.
- Browser application is closed or crashed.
- The [browser.close()](#browser-close) method was called.

## browser_context.on("page")<a name="browser-context-event-page">#</a>

- type: \<[Page](#page)>

The event is emitted when a new Page is created in the BrowserContext. The page may still be loading. The event will also fire for popup pages. See also [page.on("popup")](#page-event-popup) to receive events about popups relevant to a specific page.

The earliest moment that page is available is when it has navigated to the initial url. For example, when opening a popup with `window.open('http://example.com')`, this event will fire when the network request to "[http://example.com"](http://example.com"/) is done and its response has started loading in the popup.

- Sync

```python
with context.expect_page() as page_info:
    page.click("a[target=_blank]"),
page = page_info.value
print(page.evaluate("location.href"))
```

- Async

```python
async with context.expect_page() as page_info:
    await page.click("a[target=_blank]"),
page = await page_info.value
print(await page.evaluate("location.href"))
```

> NOTE
>
> Use [page.wait_for_load_state(**kwargs)](#page-wait-for-load-state) to wait until the page gets to a particular state (you should not need it in most cases).

## browser_context.on("request")<a name="browser-context-event-request">#</a>

- type: \<[Request](#request)>

Emitted when a request is issued from any pages created through this context. The [request](#request) object is read-only. To only listen for requests from a particular page, use [page.on("request")](#page-event-request).

In order to intercept and mutate requests, see [browser_context.route(url, handler, **kwargs)](#browser-context-route) or [page.route(url, handler, **kwargs)](#page-route).

## browser_context.on("requestfailed")<a name="browser-context-event-request-failed">#</a>

- type: \<[Request](#request)>

Emitted when a request fails, for example by timing out. To only listen for failed requests from a particular page, use [page.on("requestfailed")](#page-event-request-failed).

> NOTE
>
> HTTP Error responses, such as 404 or 503, are still successful responses from HTTP standpoint, so request will complete with [browser_context.on("requestfinished")](#browser-context-event-request-finished) event and not with [browser_context.on("requestfailed")](#browser-context-event-request-failed).

## browser_context.on("requestfinished")<a name="browser-context-event-request-finished">#</a>

- type: \<[Request](#request)>

Emitted when a request finishes successfully after downloading the response body. For a successful response, the sequence of events is `request`, `response` and `requestfinished`. To listen for successful requests from a particular page, use [page.on("requestfinished")](#page-event-request-finished).

## browser_context.on("response")<a name="browser-context-event-response">#</a>

- type: \<[Response](#response)>

Emitted when [response](#response) status and headers are received for a request. For a successful response, the sequence of events is `request`, `response` and `requestfinished`. To listen for response events from a particular page, use [page.on("response")](#page-event-response).

## browser_context.on("serviceworker")<a name="browser-context-event-service-worker">#</a>

- type: \<[Worker](#worker)>

> NOTE
>
> Service workers are only supported on Chromium-based browsers.
>
> Emitted when new service worker is created in the context.

## browser_context.add_cookies(cookies)<a name="browser-context-add-cookies">#</a>

- `cookies` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="browser-context-add-cookies-option-cookies">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> either url or domain / path are required. Optional.
    - `domain` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> either url or domain / path are required Optional.
    - `path` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> either url or domain / path are required Optional.
    - `expires` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Unix time in seconds. Optional.
    - `httpOnly` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Optional.
    - `secure` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Optional.
    - `sameSite` \<"Strict"|"Lax"|"None"> Optional.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-add-cookies-return">#</a>

Adds cookies into this browser context. All pages within this context will have these cookies installed. Cookies can be obtained via [browser_context.cookies(**kwargs)](#browser-context-cookies).

- Sync

```python
browser_context.add_cookies([cookie_object1, cookie_object2])
```

- Async

```python
await browser_context.add_cookies([cookie_object1, cookie_object2])
```

## browser_context.add_init_script(**kwargs)<a name="browser-context-add-init-script">#</a>

- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to the JavaScript file. If `path` is a relative path, then it is resolved relative to the current working directory. Optional.<a name="browser-context-add-init-script-option-path">#</a>
- `script` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Script to be evaluated in all pages in the browser context. Optional.<a name="browser-context-add-init-script-option-script">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-add-init-script-return">#</a>

Adds a script which would be evaluated in one of the following scenarios:

- Whenever a page is created in the browser context or is navigated.
- Whenever a child frame is attached or navigated in any page in the browser context. In this case, the script is evaluated in the context of the newly attached frame.

The script is evaluated after the document was created but before any of its scripts were run. This is useful to amend the JavaScript environment, e.g. to seed `Math.random`.

An example of overriding `Math.random` before the page loads:

```js
// preload.js
Math.random = () => 42;
```

- Sync

```python
# in your playwright script, assuming the preload.js file is in same directory.
browser_context.add_init_script(path="preload.js")
```

- Async

```python
# in your playwright script, assuming the preload.js file is in same directory.
await browser_context.add_init_script(path="preload.js")
```

> NOTE
>
> The order of evaluation of multiple scripts installed via [browser_context.add_init_script(**kwargs)](#browser-context-add-init-script) and [page.add_init_script(**kwargs)](#page-add-init-script) is not defined.

## browser_context.background_pages<a name="browser-context-background-pages">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Page](#page)]><a name="browser-context-background-pages-return">#</a>

> NOTE
>
> Background pages are only supported on Chromium-based browsers.
>
> All existing background pages in the context.

## browser_context.browser<a name="browser-context-browser">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Browser](#browser)><a name="browser-context-browser-return">#</a>

Returns the browser instance of the context. If it was launched as a persistent context null gets returned.

## browser_context.clear_cookies()<a name="browser-context-clear-cookies">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-clear-cookies-return">#</a>

Clears context cookies.

## browser_context.clear_permissions()<a name="browser-context-clear-permissions">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-clear-permissions-return">#</a>

Clears all permission overrides for the browser context.

- Sync

```python
context = browser.new_context()
context.grant_permissions(["clipboard-read"])
# do stuff ..
context.clear_permissions()
```

- Async

```python
context = await browser.new_context()
await context.grant_permissions(["clipboard-read"])
# do stuff ..
context.clear_permissions()
```

## browser_context.close()<a name="browser-context-close">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-close-return">#</a>

Closes the browser context. All the pages that belong to the browser context will be closed.

> NOTE
>
> The default browser context cannot be closed.

## browser_context.cookies(**kwargs)<a name="browser-context-cookies">#</a>

- `urls` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Optional list of URLs.<a name="browser-context-cookies-option-urls">#</a>
- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="browser-context-cookies-return">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `domain` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `path` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `expires` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Unix time in seconds.
    - `httpOnly` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
    - `secure` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
    - `sameSite` \<"Strict"|"Lax"|"None">

If no URLs are specified, this method returns all cookies. If URLs are specified, only cookies that affect those URLs are returned.

## browser_context.expect_event(event, **kwargs)<a name="browser-context-wait-for-event">#</a>

- `event` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Event name, same one would pass into `browserContext.on(event)`.<a name="browser-context-wait-for-event-option-event">#</a>
- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Receives the event data and resolves to truthy value when the waiting should resolve.<a name="browser-context-wait-for-event-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="browser-context-wait-for-event-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)><a name="browser-context-wait-for-event-return">#</a>

Waits for event to fire and passes its value into the predicate function. Returns when the predicate returns truthy value. Will throw an error if the context closes before the event is fired. Returns the event data value.

- Sync

```python
with context.expect_event("page") as event_info:
    page.click("button")
page = event_info.value
```

- Async

```python
async with context.expect_event("page") as event_info:
    await page.click("button")
page = await event_info.value
```

## browser_context.expect_page(**kwargs)<a name="browser-context-wait-for-page">#</a>

- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Page](#page)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Receives the [Page](#page) object and resolves to truthy value when the waiting should resolve.<a name="browser-context-wait-for-page-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="browser-context-wait-for-page-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[Page](#page)]><a name="browser-context-wait-for-page-return">#</a>

Performs action and waits for a new [Page](#page) to be created in the context. If predicate is provided, it passes [Page](#page) value into the `predicate` function and waits for `predicate(event)` to return a truthy value. Will throw an error if the context closes before new [Page](#page) is created.

## browser_context.expose_binding(name, callback, **kwargs)<a name="browser-context-expose-binding">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the function on the window object.<a name="browser-context-expose-binding-option-name">#</a>
- `callback` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Callback function that will be called in the Playwright's context.<a name="browser-context-expose-binding-option-callback">#</a>
- `handle` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to pass the argument as a handle, instead of passing by value. When passing a handle, only one argument is supported. When passing by value, multiple arguments are supported.<a name="browser-context-expose-binding-option-handle">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-expose-binding-return">#</a>

The method adds a function called `name` on the `window` object of every frame in every page in the context. When called, the function executes `callback` and returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which resolves to the return value of `callback`. If the `callback` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), it will be awaited.

The first argument of the `callback` function contains information about the caller: `{ browserContext: BrowserContext, page: Page, frame: Frame }`.

See [page.expose_binding(name, callback, **kwargs)](#page-expose-binding) for page-only version.

An example of exposing page URL to all frames in all pages in the context:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    webkit = playwright.webkit
    browser = webkit.launch(headless=false)
    context = browser.new_context()
    context.expose_binding("pageURL", lambda source: source["page"].url)
    page = context.new_page()
    page.set_content("""
    \<script>
      async function onClick() {
        document.querySelector('div').textContent = await window.pageURL();
      }
    \</script>
    \<button onclick="onClick()">Click me\</button>
    \<div>\</div>
    """)
    page.click("button")

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    webkit = playwright.webkit
    browser = await webkit.launch(headless=false)
    context = await browser.new_context()
    await context.expose_binding("pageURL", lambda source: source["page"].url)
    page = await context.new_page()
    await page.set_content("""
    \<script>
      async function onClick() {
        document.querySelector('div').textContent = await window.pageURL();
      }
    \</script>
    \<button onclick="onClick()">Click me\</button>
    \<div>\</div>
    """)
    await page.click("button")

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

An example of passing an element handle:

- Sync

```python
def print(source, element):
    print(element.text_content())

context.expose_binding("clicked", print, handle=true)
page.set_content("""
  \<script>
    document.addEventListener('click', event => window.clicked(event.target));
  \</script>
  \<div>Click me\</div>
  \<div>Or click me\</div>
""")
```

- Async

```python
async def print(source, element):
    print(await element.text_content())

await context.expose_binding("clicked", print, handle=true)
await page.set_content("""
  \<script>
    document.addEventListener('click', event => window.clicked(event.target));
  \</script>
  \<div>Click me\</div>
  \<div>Or click me\</div>
""")
```

## browser_context.expose_function(name, callback)<a name="browser-context-expose-function">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the function on the window object.<a name="browser-context-expose-function-option-name">#</a>
- `callback` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Callback function that will be called in the Playwright's context.<a name="browser-context-expose-function-option-callback">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-expose-function-return">#</a>

The method adds a function called `name` on the `window` object of every frame in every page in the context. When called, the function executes `callback` and returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which resolves to the return value of `callback`.

If the `callback` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), it will be awaited.

See [page.expose_function(name, callback)](#page-expose-function) for page-only version.

An example of adding a `sha256` function to all pages in the context:

- Sync

```python
import hashlib
from playwright.sync_api import sync_playwright

def sha256(text):
    m = hashlib.sha256()
    m.update(bytes(text, "utf8"))
    return m.hexdigest()


def run(playwright):
    webkit = playwright.webkit
    browser = webkit.launch(headless=False)
    context = browser.new_context()
    context.expose_function("sha256", sha256)
    page = context.new_page()
    page.set_content("""
        \<script>
          async function onClick() {
            document.querySelector('div').textContent = await window.sha256('PLAYWRIGHT');
          }
        \</script>
        \<button onclick="onClick()">Click me\</button>
        \<div>\</div>
    """)
    page.click("button")

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
import hashlib
from playwright.async_api import async_playwright

def sha256(text):
    m = hashlib.sha256()
    m.update(bytes(text, "utf8"))
    return m.hexdigest()


async def run(playwright):
    webkit = playwright.webkit
    browser = await webkit.launch(headless=False)
    context = await browser.new_context()
    await context.expose_function("sha256", sha256)
    page = await context.new_page()
    await page.set_content("""
        \<script>
          async function onClick() {
            document.querySelector('div').textContent = await window.sha256('PLAYWRIGHT');
          }
        \</script>
        \<button onclick="onClick()">Click me\</button>
        \<div>\</div>
    """)
    await page.click("button")

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

## browser_context.grant_permissions(permissions, **kwargs)<a name="browser-context-grant-permissions">#</a>

- `permissions` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> A permission or an array of permissions to grant. Permissions can be one of the following values:<a name="browser-context-grant-permissions-option-permissions">#</a>
    - `'geolocation'`
    - `'midi'`
    - `'midi-sysex'` (system-exclusive midi)
    - `'notifications'`
    - `'camera'`
    - `'microphone'`
    - `'background-sync'`
    - `'ambient-light-sensor'`
    - `'accelerometer'`
    - `'gyroscope'`
    - `'magnetometer'`
    - `'accessibility-events'`
    - `'clipboard-read'`
    - `'clipboard-write'`
    - `'payment-handler'`
- `origin` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> The [origin](https://developer.mozilla.org/en-US/docs/Glossary/Origin) to grant permissions to, e.g. "[https://example.com"](https://example.com"/).<a name="browser-context-grant-permissions-option-origin">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-grant-permissions-return">#</a>

Grants specified permissions to the browser context. Only grants corresponding permissions to the given origin if specified.

## browser_context.new_cdp_session(page)<a name="browser-context-new-cdp-session">#</a>

- `page` \<[Page](#page)|[Frame](#frame)> Target to create new session for. For backwards-compatibility, this parameter is named `page`, but it can be a `Page` or `Frame` type.<a name="browser-context-new-cdp-session-option-page">#</a>
- returns: \<[CDPSession](#cdpsession)><a name="browser-context-new-cdp-session-return">#</a>

> NOTE
>
> CDP sessions are only supported on Chromium-based browsers.
>
> Returns the newly created session.

## browser_context.new_page()<a name="browser-context-new-page">#</a>

- returns: \<[Page](#page)><a name="browser-context-new-page-return">#</a>

Creates a new page in the browser context.

## browser_context.pages<a name="browser-context-pages">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Page](#page)]><a name="browser-context-pages-return">#</a>

Returns all open pages in the context.

## browser_context.route(url, handler, **kwargs)<a name="browser-context-route">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[URL](https://en.wikipedia.org/wiki/URL)]:[bool](https://docs.python.org/3/library/stdtypes.html)> A glob pattern, regex pattern or predicate receiving [URL](https://en.wikipedia.org/wiki/URL) to match while routing. When a `base_url` via the context options was provided and the passed URL is a path, it gets merged via the [`new URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) constructor.<a name="browser-context-route-option-url">#</a>
- `handler` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Route](#route), [Request](#request)]> handler function to route the request.<a name="browser-context-route-option-handler">#</a>
- `times` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> How often a route should be used. By default it will be used every time.<a name="browser-context-route-option-times">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-route-return">#</a>

Routing provides the capability to modify network requests that are made by any page in the browser context. Once route is enabled, every request matching the url pattern will stall unless it's continued, fulfilled or aborted.

> NOTE
>
> [page.route(url, handler, **kwargs)](#page-route) will not intercept requests intercepted by Service Worker. See [this](https://github.com/microsoft/playwright/issues/1090) issue. We recommend disabling Service Workers when using request interception. Via `await context.addInitScript(() => delete window.navigator.serviceWorker);`

An example of a naive handler that aborts all image requests:

- Sync

```python
context = browser.new_context()
page = context.new_page()
context.route("**/*.{png,jpg,jpeg}", lambda route: route.abort())
page.goto("https://example.com")
browser.close()
```

- Async

```python
context = await browser.new_context()
page = await context.new_page()
await context.route("**/*.{png,jpg,jpeg}", lambda route: route.abort())
await page.goto("https://example.com")
await browser.close()
```

or the same snippet using a regex pattern instead:

- Sync

```python
context = browser.new_context()
page = context.new_page()
context.route(re.compile(r"(\.png$)|(\.jpg$)"), lambda route: route.abort())
page = await context.new_page()
page = context.new_page()
page.goto("https://example.com")
browser.close()
```

- Async

```python
context = await browser.new_context()
page = await context.new_page()
await context.route(re.compile(r"(\.png$)|(\.jpg$)"), lambda route: route.abort())
page = await context.new_page()
await page.goto("https://example.com")
await browser.close()
```

It is possible to examine the request to decide the route action. For example, mocking all requests that contain some post data, and leaving all other requests as is:

- Sync

```python
def handle_route(route):
  if ("my-string" in route.request.post_data)
    route.fulfill(body="mocked-data")
  else
    route.continue_()
context.route("/api/**", handle_route)
```

- Async

```python
def handle_route(route):
  if ("my-string" in route.request.post_data)
    route.fulfill(body="mocked-data")
  else
    route.continue_()
await context.route("/api/**", handle_route)
```

Page routes (set up with [page.route(url, handler, **kwargs)](https://playwright.dev/python/docs/api/class-page#page-route)) take precedence over browser context routes when request matches both handlers.

To remove a route with its handler you can use [browser_context.unroute(url, **kwargs)](#browser-context-unroute).

> NOTE
>
> Enabling routing disables http cache.

## browser_context.service_workers<a name="browser-context-service-workers">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Worker](#worker)]><a name="browser-context-service-workers-return">#</a>

> NOTE
>
> Service workers are only supported on Chromium-based browsers.
>
> All existing service workers in the context.

## browser_context.set_default_navigation_timeout(timeout)<a name="browser-context-set-default-navigation-timeout">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum navigation time in milliseconds<a name="browser-context-set-default-navigation-timeout-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-set-default-navigation-timeout-return">#</a>

This setting will change the default maximum navigation time for the following methods and related shortcuts:

- [page.go_back(**kwargs)](#page-go-back)
- [page.go_forward(**kwargs)](#page-go-forward)
- [page.goto(url, **kwargs)](#page-goto)
- [page.reload(**kwargs)](#page-reload)
- [page.set_content(html, **kwargs)](#page-set-content)
- [page.expect_navigation(**kwargs)](#page-wait-for-navigation)

> NOTE
>
> [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) and [page.set_default_timeout(timeout)](#page-set-default-timeout) take priority over [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout).

## browser_context.set_default_timeout(timeout)<a name="browser-context-set-default-timeout">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds<a name="browser-context-set-default-timeout-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-set-default-timeout-return">#</a>

This setting will change the default maximum time for all the methods accepting `timeout` option.

> NOTE
>
> [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout), [page.set_default_timeout(timeout)](#page-set-default-timeout) and [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout) take priority over [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).

## browser_context.set_extra_http_headers(headers)<a name="browser-context-set-extra-http-headers">#</a>

- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> An object containing additional HTTP headers to be sent with every request. All header values must be strings.<a name="browser-context-set-extra-http-headers-option-headers">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-set-extra-http-headers-return">#</a>

The extra HTTP headers will be sent with every request initiated by any page in the context. These headers are merged with page-specific extra HTTP headers set with [page.set_extra_http_headers(headers)](#page-set-extra-http-headers). If page overrides a particular header, page-specific header value will be used instead of the browser context header value.

> NOTE
>
> [browser_context.set_extra_http_headers(headers)](#browser-context-set-extra-http-headers) does not guarantee the order of headers in the outgoing requests.

## browser_context.set_geolocation(geolocation)<a name="browser-context-set-geolocation">#</a>

- `geolocation` \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="browser-context-set-geolocation-option-geolocation">#</a>
    - `latitude` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Latitude between -90 and 90.
    - `longitude` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Longitude between -180 and 180.
    - `accuracy` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Non-negative accuracy value. Defaults to `0`.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-set-geolocation-return">#</a>

Sets the context's geolocation. Passing `null` or `undefined` emulates position unavailable.

- Sync

```python
browser_context.set_geolocation({"latitude": 59.95, "longitude": 30.31667})
```

- Async

```python
await browser_context.set_geolocation({"latitude": 59.95, "longitude": 30.31667})
```

> NOTE
>
> Consider using [browser_context.grant_permissions(permissions, **kwargs)](#browser-context-grant-permissions) to grant permissions for the browser context pages to read its geolocation.

## browser_context.set_offline(offline)<a name="browser-context-set-offline">#</a>

- `offline` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to emulate network being offline for the browser context.<a name="browser-context-set-offline-option-offline">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-set-offline-return">#</a>

## browser_context.storage_state(**kwargs)<a name="browser-context-storage-state">#</a>

- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> The file path to save the storage state to. If `path` is a relative path, then it is resolved relative to current working directory. If no path is provided, storage state is still returned, but won't be saved to the disk.<a name="browser-context-storage-state-option-path">#</a>
- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="browser-context-storage-state-return">#</a>
    - `cookies` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
        - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `domain` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `path` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `expires` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Unix time in seconds.
        - `httpOnly` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `secure` \<[bool](https://docs.python.org/3/library/stdtypes.html)>
        - `sameSite` \<"Strict"|"Lax"|"None">
    - `origins` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
        - `origin` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
        - `localStorage` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]>
            - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
            - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

Returns storage state for this browser context, contains current cookies and local storage snapshot.

## browser_context.unroute(url, **kwargs)<a name="browser-context-unroute">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[URL](https://en.wikipedia.org/wiki/URL)]:[bool](https://docs.python.org/3/library/stdtypes.html)> A glob pattern, regex pattern or predicate receiving [URL](https://en.wikipedia.org/wiki/URL) used to register a routing with [browser_context.route(url, handler, **kwargs)](#browser-context-route).<a name="browser-context-unroute-option-url">#</a>
- `handler` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Route](#route), [Request](#request)]> Optional handler function used to register a routing with [browser_context.route(url, handler, **kwargs)](#browser-context-route).<a name="browser-context-unroute-option-handler">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="browser-context-unroute-return">#</a>

Removes a route created with [browser_context.route(url, handler, **kwargs)](#browser-context-route). When `handler` is not specified, removes all routes for the `url`.

## browser_context.wait_for_event(event, **kwargs)<a name="browser-context-wait-for-event-2">#</a>

- `event` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Event name, same one typically passed into `*.on(event)`.<a name="browser-context-wait-for-event-2-option-event">#</a>
- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Receives the event data and resolves to truthy value when the waiting should resolve.<a name="browser-context-wait-for-event-2-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="browser-context-wait-for-event-2-option-timeout">#</a>
- returns: \<[Any](https://docs.python.org/3/library/typing.html#typing.Any)><a name="browser-context-wait-for-event-2-return">#</a>

> NOTE
>
> In most cases, you should use [browser_context.expect_event(event, **kwargs)](#browser-context-wait-for-event).
>
> Waits for given `event` to fire. If predicate is provided, it passes event's value into the `predicate` function and waits for `predicate(event)` to return a truthy value. Will throw an error if the browser context is closed before the `event` is fired.

## browser_context.request<a name="browser-context-request">#</a>

- type: \<[APIRequestContext](#apirequestcontext)>

API testing helper associated with this context. Requests made with this API will use context cookies.

## browser_context.tracing<a name="browser-context-tracing">#</a>

- type: \<[Tracing](#tracing)>





# BrowserType

BrowserType provides methods to launch a specific browser instance or connect to an existing one. The following is a typical example of using Playwright to drive automation:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    chromium = playwright.chromium
    browser = chromium.launch()
    page = browser.new_page()
    page.goto("https://example.com")
    # other actions...
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    chromium = playwright.chromium
    browser = await chromium.launch()
    page = await browser.new_page()
    await page.goto("https://example.com")
    # other actions...
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

## browser_type.connect(ws_endpoint, **kwargs)<a name="browser-type-connect">#</a>

- `ws_endpoint` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A browser websocket endpoint to connect to.<a name="browser-type-connect-option-ws-endpoint">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Additional HTTP headers to be sent with web socket connect request. Optional.<a name="browser-type-connect-option-headers">#</a>
- `slow_mo` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Slows down Playwright operations by the specified amount of milliseconds. Useful so that you can see what is going on. Defaults to 0.<a name="browser-type-connect-option-slow-mo">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds to wait for the connection to be established. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="browser-type-connect-option-timeout">#</a>
- returns: \<[Browser](#browser)><a name="browser-type-connect-return">#</a>

This methods attaches Playwright to an existing browser instance.

## browser_type.connect_over_cdp(endpoint_url, **kwargs)<a name="browser-type-connect-over-cdp">#</a>

- `endpoint_url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A CDP websocket endpoint or http url to connect to. For example `http://localhost:9222/` or `ws://127.0.0.1:9222/devtools/browser/387adf4c-243f-4051-a181-46798f4a46f4`.<a name="browser-type-connect-over-cdp-option-endpoint-url">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Additional HTTP headers to be sent with connect request. Optional.<a name="browser-type-connect-over-cdp-option-headers">#</a>
- `slow_mo` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Slows down Playwright operations by the specified amount of milliseconds. Useful so that you can see what is going on. Defaults to 0.<a name="browser-type-connect-over-cdp-option-slow-mo">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds to wait for the connection to be established. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="browser-type-connect-over-cdp-option-timeout">#</a>
- returns: \<[Browser](#browser)><a name="browser-type-connect-over-cdp-return">#</a>

This methods attaches Playwright to an existing browser instance using the Chrome DevTools Protocol.

The default browser context is accessible via [browser.contexts](#browser-contexts).

> NOTE
>
> Connecting over the Chrome DevTools Protocol is only supported for Chromium-based browsers.

## browser_type.executable_path<a name="browser-type-executable-path">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="browser-type-executable-path-return">#</a>

A path where Playwright expects to find a bundled browser executable.

## browser_type.launch(**kwargs)<a name="browser-type-launch">#</a>

- `args` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Additional arguments to pass to the browser instance. The list of Chromium flags can be found [here](http://peter.sh/experiments/chromium-command-line-switches/).<a name="browser-type-launch-option-args">#</a>
- `channel` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Browser distribution channel. Supported values are "chrome", "chrome-beta", "chrome-dev", "chrome-canary", "msedge", "msedge-beta", "msedge-dev", "msedge-canary". Read more about using [Google Chrome and Microsoft Edge](https://playwright.dev/python/docs/browsers#google-chrome--microsoft-edge).<a name="browser-type-launch-option-channel">#</a>
- `chromium_sandbox` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Enable Chromium sandboxing. Defaults to `false`.<a name="browser-type-launch-option-chromium-sandbox">#</a>
- `devtools` \<[bool](https://docs.python.org/3/library/stdtypes.html)> **Chromium-only** Whether to auto-open a Developer Tools panel for each tab. If this option is `true`, the `headless` option will be set `false`.<a name="browser-type-launch-option-devtools">#</a>
- `downloads_path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> If specified, accepted downloads are downloaded into this directory. Otherwise, temporary directory is created and is deleted when browser is closed. In either case, the downloads are deleted when the browser context they were created in is closed.<a name="browser-type-launch-option-downloads-path">#</a>
- `env` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Specify environment variables that will be visible to the browser. Defaults to `process.env`.<a name="browser-type-launch-option-env">#</a>
- `executable_path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to a browser executable to run instead of the bundled one. If `executable_path` is a relative path, then it is resolved relative to the current working directory. Note that Playwright only works with the bundled Chromium, Firefox or WebKit, use at your own risk.<a name="browser-type-launch-option-executable-path">#</a>
- `firefox_user_prefs` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Firefox user preferences. Learn more about the Firefox user preferences at [`about:config`](https://support.mozilla.org/en-US/kb/about-config-editor-firefox).<a name="browser-type-launch-option-firefox-user-prefs">#</a>
- `handle_sighup` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Close the browser process on SIGHUP. Defaults to `true`.<a name="browser-type-launch-option-handle-sighup">#</a>
- `handle_sigint` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Close the browser process on Ctrl-C. Defaults to `true`.<a name="browser-type-launch-option-handle-sigint">#</a>
- `handle_sigterm` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Close the browser process on SIGTERM. Defaults to `true`.<a name="browser-type-launch-option-handle-sigterm">#</a>
- `headless` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to run browser in headless mode. More details for [Chromium](https://developers.google.com/web/updates/2017/04/headless-chrome) and [Firefox](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Headless_mode). Defaults to `true` unless the `devtools` option is `true`.<a name="browser-type-launch-option-headless">#</a>
- `ignore_default_args` \<[bool](https://docs.python.org/3/library/stdtypes.html)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> If `true`, Playwright does not pass its own configurations args and only uses the ones from `args`. If an array is given, then filters out the given default arguments. Dangerous option; use with care. Defaults to `false`.<a name="browser-type-launch-option-ignore-default-args">#</a>
- `proxy` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Network proxy settings.<a name="browser-type-launch-option-proxy">#</a>
    - `server` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Proxy to be used for all requests. HTTP and SOCKS proxies are supported, for example `http://myproxy.com:3128` or `socks5://myproxy.com:3128`. Short form `myproxy.com:3128` is considered an HTTP proxy.
    - `bypass` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional comma-separated domains to bypass proxy, for example `".com, chromium.org, .domain.com"`.
    - `username` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional username to use if HTTP proxy requires authentication.
    - `password` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional password to use if HTTP proxy requires authentication.
- `slow_mo` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Slows down Playwright operations by the specified amount of milliseconds. Useful so that you can see what is going on.<a name="browser-type-launch-option-slow-mo">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds to wait for the browser instance to start. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="browser-type-launch-option-timeout">#</a>
- `traces_dir` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> If specified, traces are saved into this directory.<a name="browser-type-launch-option-traces-dir">#</a>
- returns: \<[Browser](#browser)><a name="browser-type-launch-return">#</a>

Returns the browser instance.

You can use `ignore_default_args` to filter out `--mute-audio` from default arguments:

- Sync

```python
browser = playwright.chromium.launch( # or "firefox" or "webkit".
    ignore_default_args=["--mute-audio"]
)
```

- Async

```python
browser = await playwright.chromium.launch( # or "firefox" or "webkit".
    ignore_default_args=["--mute-audio"]
)
```

> **Chromium-only** Playwright can also be used to control the Google Chrome or Microsoft Edge browsers, but it works best with the version of Chromium it is bundled with. There is no guarantee it will work with any other version. Use `executable_path` option with extreme caution.
>
> If Google Chrome (rather than Chromium) is preferred, a [Chrome Canary](https://www.google.com/chrome/browser/canary.html) or [Dev Channel](https://www.chromium.org/getting-involved/dev-channel) build is suggested.
>
> Stock browsers like Google Chrome and Microsoft Edge are suitable for tests that require proprietary media codecs for video playback. See [this article](https://www.howtogeek.com/202825/what’s-the-difference-between-chromium-and-chrome/) for other differences between Chromium and Chrome. [This article](https://chromium.googlesource.com/chromium/src/+/lkgr/docs/chromium_browser_vs_google_chrome.md) describes some differences for Linux users.

## browser_type.launch_persistent_context(user_data_dir, **kwargs)<a name="browser-type-launch-persistent-context">#</a>

- `user_data_dir` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to a User Data Directory, which stores browser session data like cookies and local storage. More details for [Chromium](https://chromium.googlesource.com/chromium/src/+/master/docs/user_data_dir.md#introduction) and [Firefox](https://developer.mozilla.org/en-US/docs/Mozilla/Command_Line_Options#User_Profile). Note that Chromium's user data directory is the **parent** directory of the "Profile Path" seen at `chrome://version`. Pass an empty string to use a temporary directory instead.<a name="browser-type-launch-persistent-context-option-user-data-dir">#</a>

- `accept_downloads` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to automatically download all the attachments. Defaults to `true` where all the downloads are accepted.<a name="browser-type-launch-persistent-context-option-accept-downloads">#</a>

- `args` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Additional arguments to pass to the browser instance. The list of Chromium flags can be found [here](http://peter.sh/experiments/chromium-command-line-switches/).<a name="browser-type-launch-persistent-context-option-args">#</a>

- `base_url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> When using [page.goto(url, **kwargs)](#page-goto), [page.route(url, handler, **kwargs)](#page-route), [page.wait_for_url(url, **kwargs)](#page-wait-for-url), [page.expect_request(url_or_predicate, **kwargs)](#page-wait-for-request), or [page.expect_response(url_or_predicate, **kwargs)](#page-wait-for-response) it takes the base URL in consideration by using the [`URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) constructor for building the corresponding URL. Examples:<a name="browser-type-launch-persistent-context-option-base-url">#</a>

    - baseURL: `http://localhost:3000` and navigating to `/bar.html` results in `http://localhost:3000/bar.html`
    - baseURL: `http://localhost:3000/foo/` and navigating to `./bar.html` results in `http://localhost:3000/foo/bar.html`
    - baseURL: `http://localhost:3000/foo` (without trailing slash) and navigating to `./bar.html` results in `http://localhost:3000/bar.html`

- `bypass_csp` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Toggles bypassing page's Content-Security-Policy.<a name="browser-type-launch-persistent-context-option-bypass-csp">#</a>

- `channel` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Browser distribution channel. Supported values are "chrome", "chrome-beta", "chrome-dev", "chrome-canary", "msedge", "msedge-beta", "msedge-dev", "msedge-canary". Read more about using [Google Chrome and Microsoft Edge](https://playwright.dev/python/docs/browsers#google-chrome--microsoft-edge).<a name="browser-type-launch-persistent-context-option-channel">#</a>

- `chromium_sandbox` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Enable Chromium sandboxing. Defaults to `false`.<a name="browser-type-launch-persistent-context-option-chromium-sandbox">#</a>

- `color_scheme` \<"light"|"dark"|"no-preference"> Emulates `'prefers-colors-scheme'` media feature, supported values are `'light'`, `'dark'`, `'no-preference'`. See [page.emulate_media(**kwargs)](#page-emulate-media) for more details. Defaults to `'light'`.<a name="browser-type-launch-persistent-context-option-color-scheme">#</a>

- `device_scale_factor` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Specify device scale factor (can be thought of as dpr). Defaults to `1`.<a name="browser-type-launch-persistent-context-option-device-scale-factor">#</a>

- `devtools` \<[bool](https://docs.python.org/3/library/stdtypes.html)> **Chromium-only** Whether to auto-open a Developer Tools panel for each tab. If this option is `true`, the `headless` option will be set `false`.<a name="browser-type-launch-persistent-context-option-devtools">#</a>

- `downloads_path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> If specified, accepted downloads are downloaded into this directory. Otherwise, temporary directory is created and is deleted when browser is closed. In either case, the downloads are deleted when the browser context they were created in is closed.<a name="browser-type-launch-persistent-context-option-downloads-path">#</a>

- `env` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[bool](https://docs.python.org/3/library/stdtypes.html)]> Specify environment variables that will be visible to the browser. Defaults to `process.env`.<a name="browser-type-launch-persistent-context-option-env">#</a>

- `executable_path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to a browser executable to run instead of the bundled one. If `executable_path` is a relative path, then it is resolved relative to the current working directory. Note that Playwright only works with the bundled Chromium, Firefox or WebKit, use at your own risk.<a name="browser-type-launch-persistent-context-option-executable-path">#</a>

- `extra_http_headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> An object containing additional HTTP headers to be sent with every request.<a name="browser-type-launch-persistent-context-option-extra-http-headers">#</a>

- `forced_colors` \<"active"|"none"> Emulates `'forced-colors'` media feature, supported values are `'active'`, `'none'`. See [page.emulate_media(**kwargs)](#page-emulate-media) for more details. Defaults to `'none'`.<a name="browser-type-launch-persistent-context-option-forced-colors">#</a>

    > NOTE
    >
    > It's not supported in WebKit, see [here](https://bugs.webkit.org/show_bug.cgi?id=225281) in their issue tracker.

- `geolocation` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="browser-type-launch-persistent-context-option-geolocation">#</a>

    - `latitude` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Latitude between -90 and 90.
    - `longitude` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Longitude between -180 and 180.
    - `accuracy` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Non-negative accuracy value. Defaults to `0`.

- `handle_sighup` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Close the browser process on SIGHUP. Defaults to `true`.<a name="browser-type-launch-persistent-context-option-handle-sighup">#</a>

- `handle_sigint` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Close the browser process on Ctrl-C. Defaults to `true`.<a name="browser-type-launch-persistent-context-option-handle-sigint">#</a>

- `handle_sigterm` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Close the browser process on SIGTERM. Defaults to `true`.<a name="browser-type-launch-persistent-context-option-handle-sigterm">#</a>

- `has_touch` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Specifies if viewport supports touch events. Defaults to false.<a name="browser-type-launch-persistent-context-option-has-touch">#</a>

- `headless` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to run browser in headless mode. More details for [Chromium](https://developers.google.com/web/updates/2017/04/headless-chrome) and [Firefox](https://developer.mozilla.org/en-US/docs/Mozilla/Firefox/Headless_mode). Defaults to `true` unless the `devtools` option is `true`.<a name="browser-type-launch-persistent-context-option-headless">#</a>

- `http_credentials` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Credentials for [HTTP authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication).<a name="browser-type-launch-persistent-context-option-http-credentials">#</a>

    - `username` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>
    - `password` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

- `ignore_default_args` \<[bool](https://docs.python.org/3/library/stdtypes.html)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> If `true`, Playwright does not pass its own configurations args and only uses the ones from `args`. If an array is given, then filters out the given default arguments. Dangerous option; use with care. Defaults to `false`.<a name="browser-type-launch-persistent-context-option-ignore-default-args">#</a>

- `ignore_https_errors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to ignore HTTPS errors when sending network requests. Defaults to `false`.<a name="browser-type-launch-persistent-context-option-ignore-https-errors">#</a>

- `is_mobile` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether the `meta viewport` tag is taken into account and touch events are enabled. Defaults to `false`. Not supported in Firefox.<a name="browser-type-launch-persistent-context-option-is-mobile">#</a>

- `java_script_enabled` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether or not to enable JavaScript in the context. Defaults to `true`.<a name="browser-type-launch-persistent-context-option-java-script-enabled">#</a>

- `locale` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Specify user locale, for example `en-GB`, `de-DE`, etc. Locale will affect `navigator.language` value, `Accept-Language` request header value as well as number and date formatting rules.<a name="browser-type-launch-persistent-context-option-locale">#</a>

- `no_viewport` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Does not enforce fixed viewport, allows resizing window in the headed mode.<a name="browser-type-launch-persistent-context-option-no-viewport">#</a>

- `offline` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to emulate network being offline. Defaults to `false`.<a name="browser-type-launch-persistent-context-option-offline">#</a>

- `permissions` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> A list of permissions to grant to all pages in this context. See [browser_context.grant_permissions(permissions, **kwargs)](#browser-context-grant-permissions) for more details.<a name="browser-type-launch-persistent-context-option-permissions">#</a>

- `proxy` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Network proxy settings.<a name="browser-type-launch-persistent-context-option-proxy">#</a>

    - `server` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Proxy to be used for all requests. HTTP and SOCKS proxies are supported, for example `http://myproxy.com:3128` or `socks5://myproxy.com:3128`. Short form `myproxy.com:3128` is considered an HTTP proxy.
    - `bypass` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional comma-separated domains to bypass proxy, for example `".com, chromium.org, .domain.com"`.
    - `username` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional username to use if HTTP proxy requires authentication.
    - `password` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional password to use if HTTP proxy requires authentication.

- `record_har_omit_content` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Optional setting to control whether to omit request content from the HAR. Defaults to `false`.<a name="browser-type-launch-persistent-context-option-record-har-omit-content">#</a>

- `record_har_path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Enables [HAR](http://www.softwareishard.com/blog/har-12-spec) recording for all pages into the specified HAR file on the filesystem. If not specified, the HAR is not recorded. Make sure to call [browser_context.close()](#browser-context-close) for the HAR to be saved.<a name="browser-type-launch-persistent-context-option-record-har-path">#</a>

- `record_video_dir` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Enables video recording for all pages into the specified directory. If not specified videos are not recorded. Make sure to call [browser_context.close()](#browser-context-close) for videos to be saved.<a name="browser-type-launch-persistent-context-option-record-video-dir">#</a>

- `record_video_size` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Dimensions of the recorded videos. If not specified the size will be equal to `viewport` scaled down to fit into 800x800. If `viewport` is not configured explicitly the video size defaults to 800x450. Actual picture of each page will be scaled down if necessary to fit the specified size.<a name="browser-type-launch-persistent-context-option-record-video-size">#</a>

    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Video frame width.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Video frame height.

- `reduced_motion` \<"reduce"|"no-preference"> Emulates `'prefers-reduced-motion'` media feature, supported values are `'reduce'`, `'no-preference'`. See [page.emulate_media(**kwargs)](#page-emulate-media) for more details. Defaults to `'no-preference'`.<a name="browser-type-launch-persistent-context-option-reduced-motion">#</a>

- `screen` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Emulates consistent window screen size available inside web page via `window.screen`. Is only used when the `viewport` is set.

    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page width in pixels.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page height in pixels.

- `slow_mo` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Slows down Playwright operations by the specified amount of milliseconds. Useful so that you can see what is going on.<a name="browser-type-launch-persistent-context-option-slow-mo">#</a>

- `strict_selectors` \<[bool](https://docs.python.org/3/library/stdtypes.html)> It specified, enables strict selectors mode for this context. In the strict selectors mode all operations on selectors that imply single target DOM element will throw when more than one element matches the selector. See [Locator](#locator) to learn more about the strict mode.<a name="browser-type-launch-persistent-context-option-strict-selectors">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds to wait for the browser instance to start. Defaults to `30000` (30 seconds). Pass `0` to disable timeout.<a name="browser-type-launch-persistent-context-option-timeout">#</a>

- `timezone_id` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Changes the timezone of the context. See [ICU's metaZones.txt](https://cs.chromium.org/chromium/src/third_party/icu/source/data/misc/metaZones.txt?rcl=faee8bc70570192d82d2978a71e2a615788597d1) for a list of supported timezone IDs.<a name="browser-type-launch-persistent-context-option-timezone-id">#</a>

- `traces_dir` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> If specified, traces are saved into this directory.<a name="browser-type-launch-persistent-context-option-traces-dir">#</a>

- `user_agent` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Specific user agent to use in this context.<a name="browser-type-launch-persistent-context-option-user-agent">#</a>

- `viewport` \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Sets a consistent viewport for each page. Defaults to an 1280x720 viewport. `no_viewport` disables the fixed viewport.<a name="browser-type-launch-persistent-context-option-viewport">#</a>

    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page width in pixels.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page height in pixels.

- returns: \<[BrowserContext](#browsercontext)><a name="browser-type-launch-persistent-context-return">#</a>

Returns the persistent browser context instance.

Launches browser that uses persistent storage located at `user_data_dir` and returns the only context. Closing this context will automatically close the browser.

## browser_type.name<a name="browser-type-name">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="browser-type-name-return">#</a>

Returns browser name. For example: `'chromium'`, `'webkit'` or `'firefox'`.





# CDPSession

- extends: [EventEmitter](https://pyee.readthedocs.io/en/latest/#pyee.BaseEventEmitter)

The `CDPSession` instances are used to talk raw Chrome Devtools Protocol:

- protocol methods can be called with `session.send` method.
- protocol events can be subscribed to with `session.on` method.

Useful links:

- Documentation on DevTools Protocol can be found here: [DevTools Protocol Viewer](https://chromedevtools.github.io/devtools-protocol/).
- Getting Started with DevTools Protocol: https://github.com/aslushnikov/getting-started-with-cdp/blob/master/README.md

- Sync

```python
client = page.context.new_cdp_session(page)
client.send("Animation.enable")
client.on("Animation.animationCreated", lambda: print("animation created!"))
response = client.send("Animation.getPlaybackRate")
print("playback rate is " + str(response["playbackRate"]))
client.send("Animation.setPlaybackRate", {
    playbackRate: response["playbackRate"] / 2
})
```

- Async

```python
client = await page.context.new_cdp_session(page)
await client.send("Animation.enable")
client.on("Animation.animationCreated", lambda: print("animation created!"))
response = await client.send("Animation.getPlaybackRate")
print("playback rate is " + str(response["playbackRate"]))
await client.send("Animation.setPlaybackRate", {
    playbackRate: response["playbackRate"] / 2
})
```

- [cdp_session.detach()](#cdp-session-detach)
- [cdp_session.send(method, **kwargs)](#cdp-session-send)

## cdp_session.detach()<a name="cdp-session-detach">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="cdp-session-detach-return">#</a>

Detaches the CDPSession from the target. Once detached, the CDPSession object won't emit any events and can't be used to send messages.

## cdp_session.send(method, **kwargs)<a name="cdp-session-send">#</a>

- `method` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> protocol method name<a name="cdp-session-send-option-method">#</a>
- `params` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Optional method parameters<a name="cdp-session-send-option-params">#</a>
- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="cdp-session-send-return">#</a>





# ConsoleMessage

[ConsoleMessage](#consolemessage) objects are dispatched by page via the [page.on("console")](#page-event-console) event.

- [console_message.args](#console-message-args)
- [console_message.location](#console-message-location)
- [console_message.text](#console-message-text)
- [console_message.type](#console-message-type)

## console_message.args<a name="console-message-args">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[JSHandle](#jshandle)]><a name="console-message-args-return">#</a>

List of arguments passed to a `console` function call. See also [page.on("console")](#page-event-console).

## console_message.location<a name="console-message-location">#</a>

- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="console-message-location-return">#</a>
    - `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> URL of the resource.
    - `lineNumber` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> 0-based line number in the resource.
    - `columnNumber` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> 0-based column number in the resource.

## console_message.text<a name="console-message-text">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="console-message-text-return">#</a>

The text of the console message.

## console_message.type<a name="console-message-type">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="console-message-type-return">#</a>

One of the following values: `'log'`, `'debug'`, `'info'`, `'error'`, `'warning'`, `'dir'`, `'dirxml'`, `'table'`, `'trace'`, `'clear'`, `'startGroup'`, `'startGroupCollapsed'`, `'endGroup'`, `'assert'`, `'profile'`, `'profileEnd'`, `'count'`, `'timeEnd'`.





# Dialog

[Dialog](#dialog) objects are dispatched by page via the [page.on("dialog")](#page-event-dialog) event.

An example of using `Dialog` class:

- Sync

```python
from playwright.sync_api import sync_playwright

def handle_dialog(dialog):
    print(dialog.message)
    dialog.dismiss()

def run(playwright):
    chromium = playwright.chromium
    browser = chromium.launch()
    page = browser.new_page()
    page.on("dialog", handle_dialog)
    page.evaluate("alert('1')")
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def handle_dialog(dialog):
    print(dialog.message)
    await dialog.dismiss()

async def run(playwright):
    chromium = playwright.chromium
    browser = await chromium.launch()
    page = await browser.new_page()
    page.on("dialog", handle_dialog)
    page.evaluate("alert('1')")
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

> NOTE
>
> Dialogs are dismissed automatically, unless there is a [page.on("dialog")](#page-event-dialog) listener. When listener is present, it **must** either [dialog.accept(**kwargs)](#dialog-accept) or [dialog.dismiss()](#dialog-dismiss) the dialog - otherwise the page will [freeze](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop#never_blocking) waiting for the dialog, and actions like click will never finish.

- [dialog.accept(**kwargs)](#dialog-accept)
- [dialog.default_value](#dialog-default-value)
- [dialog.dismiss()](#dialog-dismiss)
- [dialog.message](#dialog-message)
- [dialog.type](#dialog-type)

## dialog.accept(**kwargs)<a name="dialog-accept">#</a>

- `prompt_text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A text to enter in prompt. Does not cause any effects if the dialog's `type` is not prompt. Optional.<a name="dialog-accept-option-prompt-text">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="dialog-accept-return">#</a>

Returns when the dialog has been accepted.

## dialog.default_value<a name="dialog-default-value">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="dialog-default-value-return">#</a>

If dialog is prompt, returns default prompt value. Otherwise, returns empty string.

## dialog.dismiss()<a name="dialog-dismiss">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="dialog-dismiss-return">#</a>

Returns when the dialog has been dismissed.

## dialog.message<a name="dialog-message">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="dialog-message-return">#</a>

A message displayed in the dialog.

## dialog.type<a name="dialog-type">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="dialog-type-return">#</a>

Returns dialog's type, can be one of `alert`, `beforeunload`, `confirm` or `prompt`.



# Download

[Download](#download) objects are dispatched by page via the [page.on("download")](#page-event-download) event.

All the downloaded files belonging to the browser context are deleted when the browser context is closed.

Download event is emitted once the download starts. Download path becomes available once download completes:

- Sync

```python
with page.expect_download() as download_info:
    page.click("a")
download = download_info.value
# wait for download to complete
path = download.path()
```

- Async

```python
async with page.expect_download() as download_info:
    await page.click("a")
download = await download_info.value
# waits for download to complete
path = await download.path()
```

- [download.cancel()](#download-cancel)
- [download.delete()](#download-delete)
- [download.failure()](#download-failure)
- [download.page](#download-page)
- [download.path()](#download-path)
- [download.save_as(path)](#download-save-as)
- [download.suggested_filename](#download-suggested-filename)
- [download.url](#download-url)

## download.cancel()<a name="download-cancel">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="download-cancel-return">#</a>

Cancels a download. Will not fail if the download is already finished or canceled. Upon successful cancellations, `download.failure()` would resolve to `'canceled'`.

## download.delete()<a name="download-delete">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="download-delete-return">#</a>

Deletes the downloaded file. Will wait for the download to finish if necessary.

## download.failure()<a name="download-failure">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="download-failure-return">#</a>

Returns download error if any. Will wait for the download to finish if necessary.

## download.page<a name="download-page">#</a>

- returns: \<[Page](#page)><a name="download-page-return">#</a>

Get the page that the download belongs to.

## download.path()<a name="download-path">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[pathlib.Path](https://realpython.com/python-pathlib/)><a name="download-path-return">#</a>

Returns path to the downloaded file in case of successful download. The method will wait for the download to finish if necessary. The method throws when connected remotely.

Note that the download's file name is a random GUID, use [download.suggested_filename](#download-suggested-filename) to get suggested file name.

## download.save_as(path)<a name="download-save-as">#</a>

- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path where the download should be copied.<a name="download-save-as-option-path">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="download-save-as-return">#</a>

Copy the download to a user-specified path. It is safe to call this method while the download is still in progress. Will wait for the download to finish if necessary.

## download.suggested_filename<a name="download-suggested-filename">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="download-suggested-filename-return">#</a>

Returns suggested filename for this download. It is typically computed by the browser from the [`Content-Disposition`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Disposition) response header or the `download` attribute. See the spec on [whatwg](https://html.spec.whatwg.org/#downloading-resources). Different browsers can use different logic for computing it.

## download.url<a name="download-url">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="download-url-return">#</a>

Returns downloaded url.





# ElementHandle

- extends: [JSHandle](#jshandle)

ElementHandle represents an in-page DOM element. ElementHandles can be created with the [page.query_selector(selector, **kwargs)](#page-query-selector) method.

##### DISCOURAGED

The use of ElementHandle is discouraged, use [Locator](#locator) objects and web-first assertions instead.

- Sync

```python
href_element = page.query_selector("a")
href_element.click()
```

- Async

```python
href_element = await page.query_selector("a")
await href_element.click()
```

ElementHandle prevents DOM element from garbage collection unless the handle is disposed with [js_handle.dispose()](#js-handle-dispose). ElementHandles are auto-disposed when their origin frame gets navigated.

ElementHandle instances can be used as an argument in [page.eval_on_selector(selector, expression, **kwargs)](#page-eval-on-selector) and [page.evaluate(expression, **kwargs)](#page-evaluate) methods.

The difference between the [Locator](#locator) and ElementHandle is that the ElementHandle points to a particular element, while [Locator](#locator) captures the logic of how to retrieve an element.

In the example below, handle points to a particular DOM element on page. If that element changes text or is used by React to render an entirely different component, handle is still pointing to that very DOM element. This can lead to unexpected behaviors.

- Sync

```python
handle = page.query_selector("text=Submit")
handle.hover()
handle.click()
```

- Async

```python
handle = await page.query_selector("text=Submit")
await handle.hover()
await handle.click()
```

With the locator, every time the `element` is used, up-to-date DOM element is located in the page using the selector. So in the snippet below, underlying DOM element is going to be located twice.

- Sync

```python
locator = page.locator("text=Submit")
locator.hover()
locator.click()
```

- Async

```python
locator = page.locator("text=Submit")
await locator.hover()
await locator.click()
```

- [element_handle.bounding_box()](#element-handle-bounding-box)
- [element_handle.check(**kwargs)](#element-handle-check)
- [element_handle.click(**kwargs)](#element-handle-click)
- [element_handle.content_frame()](#element-handle-content-frame)
- [element_handle.dblclick(**kwargs)](#element-handle-dblclick)
- [element_handle.dispatch_event(type, **kwargs)](#element-handle-dispatch-event)
- [element_handle.eval_on_selector(selector, expression, **kwargs)](#element-handle-eval-on-selector)
- [element_handle.eval_on_selector_all(selector, expression, **kwargs)](#element-handle-eval-on-selector-all)
- [element_handle.fill(value, **kwargs)](#element-handle-fill)
- [element_handle.focus()](#element-handle-focus)
- [element_handle.get_attribute(name)](#element-handle-get-attribute)
- [element_handle.hover(**kwargs)](#element-handle-hover)
- [element_handle.inner_html()](#element-handle-inner-html)
- [element_handle.inner_text()](#element-handle-inner-text)
- [element_handle.input_value(**kwargs)](#element-handle-input-value)
- [element_handle.is_checked()](#element-handle-is-checked)
- [element_handle.is_disabled()](#element-handle-is-disabled)
- [element_handle.is_editable()](#element-handle-is-editable)
- [element_handle.is_enabled()](#element-handle-is-enabled)
- [element_handle.is_hidden()](#element-handle-is-hidden)
- [element_handle.is_visible()](#element-handle-is-visible)
- [element_handle.owner_frame()](#element-handle-owner-frame)
- [element_handle.press(key, **kwargs)](#element-handle-press)
- [element_handle.query_selector(selector)](#element-handle-query-selector)
- [element_handle.query_selector_all(selector)](#element-handle-query-selector-all)
- [element_handle.screenshot(**kwargs)](#element-handle-screenshot)
- [element_handle.scroll_into_view_if_needed(**kwargs)](#element-handle-scroll-into-view-if-needed)
- [element_handle.select_option(**kwargs)](#element-handle-select-option)
- [element_handle.select_text(**kwargs)](#element-handle-select-text)
- [element_handle.set_checked(checked, **kwargs)](#element-handle-set-checked)
- [element_handle.set_input_files(files, **kwargs)](#element-handle-set-input-files)
- [element_handle.tap(**kwargs)](#element-handle-tap)
- [element_handle.text_content()](#element-handle-text-content)
- [element_handle.type(text, **kwargs)](#element-handle-type)
- [element_handle.uncheck(**kwargs)](#element-handle-uncheck)
- [element_handle.wait_for_element_state(state, **kwargs)](#element-handle-wait-for-element-state)
- [element_handle.wait_for_selector(selector, **kwargs)](#element-handle-wait-for-selector)
- [js_handle.as_element()](#js-handle-as-element)
- [js_handle.dispose()](#js-handle-dispose)
- [js_handle.evaluate(expression, **kwargs)](#js-handle-evaluate)
- [js_handle.evaluate_handle(expression, **kwargs)](#js-handle-evaluate-handle)
- [js_handle.get_properties()](#js-handle-get-properties)
- [js_handle.get_property(property_name)](#js-handle-get-property)
- [js_handle.json_value()](#js-handle-json-value)

## element_handle.bounding_box()<a name="element-handle-bounding-box">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="element-handle-bounding-box-return">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> the x coordinate of the element in pixels.
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> the y coordinate of the element in pixels.
    - `width` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> the width of the element in pixels.
    - `height` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> the height of the element in pixels.

This method returns the bounding box of the element, or `null` if the element is not visible. The bounding box is calculated relative to the main frame viewport - which is usually the same as the browser window.

Scrolling affects the returned bonding box, similarly to [Element.getBoundingClientRect](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect). That means `x` and/or `y` may be negative.

Elements from child frames return the bounding box relative to the main frame, unlike the [Element.getBoundingClientRect](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect).

Assuming the page is static, it is safe to use bounding box coordinates to perform input. For example, the following snippet should click the center of the element.

- Sync

```python
box = element_handle.bounding_box()
page.mouse.click(box["x"] + box["width"] / 2, box["y"] + box["height"] / 2)
```

- Async

```python
box = await element_handle.bounding_box()
await page.mouse.click(box["x"] + box["width"] / 2, box["y"] + box["height"] / 2)
```

## element_handle.check(**kwargs)<a name="element-handle-check">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-check-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-check-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="element-handle-check-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-check-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="element-handle-check-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-check-return">#</a>

This method checks the element by performing the following steps:

1. Ensure that element is a checkbox or a radio input. If not, this method throws. If the element is already checked, this method returns immediately.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to click in the center of the element.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
6. Ensure that the element is now checked. If not, this method throws.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## element_handle.click(**kwargs)<a name="element-handle-click">#</a>

- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="element-handle-click-option-button">#</a>
- `click_count` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> defaults to 1. See [UIEvent.detail](https://developer.mozilla.org/en-US/docs/Web/API/UIEvent/detail).<a name="element-handle-click-option-click-count">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="element-handle-click-option-delay">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-click-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="element-handle-click-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-click-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="element-handle-click-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-click-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="element-handle-click-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-click-return">#</a>

This method clicks the element by performing the following steps:

1. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
2. Scroll the element into view if needed.
3. Use [page.mouse](#page-mouse) to click in the center of the element, or the specified `position`.
4. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## element_handle.content_frame()<a name="element-handle-content-frame">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Frame](#frame)><a name="element-handle-content-frame-return">#</a>

Returns the content frame for element handles referencing iframe nodes, or `null` otherwise

## element_handle.dblclick(**kwargs)<a name="element-handle-dblclick">#</a>

- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="element-handle-dblclick-option-button">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="element-handle-dblclick-option-delay">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-dblclick-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="element-handle-dblclick-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-dblclick-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="element-handle-dblclick-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-dblclick-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="element-handle-dblclick-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-dblclick-return">#</a>

This method double clicks the element by performing the following steps:

1. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
2. Scroll the element into view if needed.
3. Use [page.mouse](#page-mouse) to double click in the center of the element, or the specified `position`.
4. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set. Note that if the first click of the `dblclick()` triggers a navigation event, this method will throw.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

> NOTE
>
> `elementHandle.dblclick()` dispatches two `click` events and a single `dblclick` event.

## element_handle.dispatch_event(type, **kwargs)<a name="element-handle-dispatch-event">#</a>

- `type` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> DOM event type: `"click"`, `"dragstart"`, etc.<a name="element-handle-dispatch-event-option-type">#</a>
- `event_init` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional event-specific initialization properties.<a name="element-handle-dispatch-event-option-event-init">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-dispatch-event-return">#</a>

The snippet below dispatches the `click` event on the element. Regardless of the visibility state of the element, `click` is dispatched. This is equivalent to calling [element.click()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/click).

- Sync

```python
element_handle.dispatch_event("click")
```

- Async

```python
await element_handle.dispatch_event("click")
```

Under the hood, it creates an instance of an event based on the given `type`, initializes it with `event_init` properties and dispatches it on the element. Events are `composed`, `cancelable` and bubble by default.

Since `event_init` is event-specific, please refer to the events documentation for the lists of initial properties:

- [DragEvent](https://developer.mozilla.org/en-US/docs/Web/API/DragEvent/DragEvent)
- [FocusEvent](https://developer.mozilla.org/en-US/docs/Web/API/FocusEvent/FocusEvent)
- [KeyboardEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/KeyboardEvent)
- [MouseEvent](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/MouseEvent)
- [PointerEvent](https://developer.mozilla.org/en-US/docs/Web/API/PointerEvent/PointerEvent)
- [TouchEvent](https://developer.mozilla.org/en-US/docs/Web/API/TouchEvent/TouchEvent)
- [Event](https://developer.mozilla.org/en-US/docs/Web/API/Event/Event)

You can also specify `JSHandle` as the property value if you want live objects to be passed into the event:

- Sync

```python
# note you can only create data_transfer in chromium and firefox
data_transfer = page.evaluate_handle("new DataTransfer()")
element_handle.dispatch_event("#source", "dragstart", {"dataTransfer": data_transfer})
```

- Async

```python
# note you can only create data_transfer in chromium and firefox
data_transfer = await page.evaluate_handle("new DataTransfer()")
await element_handle.dispatch_event("#source", "dragstart", {"dataTransfer": data_transfer})
```

## element_handle.eval_on_selector(selector, expression, **kwargs)<a name="element-handle-eval-on-selector">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="element-handle-eval-on-selector-option-selector">#</a>
- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="element-handle-eval-on-selector-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="element-handle-eval-on-selector-option-arg">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="element-handle-eval-on-selector-return">#</a>

Returns the return value of `expression`.

The method finds an element matching the specified selector in the `ElementHandle`s subtree and passes it as a first argument to `expression`. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more details. If no elements match the selector, the method throws an error.

If `expression` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [element_handle.eval_on_selector(selector, expression, **kwargs)](#element-handle-eval-on-selector) would wait for the promise to resolve and return its value.

Examples:

- Sync

```python
tweet_handle = page.query_selector(".tweet")
assert tweet_handle.eval_on_selector(".like", "node => node.innerText") == "100"
assert tweet_handle.eval_on_selector(".retweets", "node => node.innerText") = "10"
```

- Async

```python
tweet_handle = await page.query_selector(".tweet")
assert await tweet_handle.eval_on_selector(".like", "node => node.innerText") == "100"
assert await tweet_handle.eval_on_selector(".retweets", "node => node.innerText") = "10"
```

## element_handle.eval_on_selector_all(selector, expression, **kwargs)<a name="element-handle-eval-on-selector-all">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="element-handle-eval-on-selector-all-option-selector">#</a>
- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="element-handle-eval-on-selector-all-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="element-handle-eval-on-selector-all-option-arg">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="element-handle-eval-on-selector-all-return">#</a>

Returns the return value of `expression`.

The method finds all elements matching the specified selector in the `ElementHandle`'s subtree and passes an array of matched elements as a first argument to `expression`. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more details.

If `expression` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [element_handle.eval_on_selector_all(selector, expression, **kwargs)](#element-handle-eval-on-selector-all) would wait for the promise to resolve and return its value.

Examples:

```html
\<div class="feed">
  \<div class="tweet">Hello!\</div>
  \<div class="tweet">Hi!\</div>
\</div>
```

- Sync

```python
feed_handle = page.query_selector(".feed")
assert feed_handle.eval_on_selector_all(".tweet", "nodes => nodes.map(n => n.innerText)") == ["hello!", "hi!"]
```

- Async

```python
feed_handle = await page.query_selector(".feed")
assert await feed_handle.eval_on_selector_all(".tweet", "nodes => nodes.map(n => n.innerText)") == ["hello!", "hi!"]
```

## element_handle.fill(value, **kwargs)<a name="element-handle-fill">#</a>

- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Value to set for the `\<input>`, `\<textarea>` or `[contenteditable]` element.<a name="element-handle-fill-option-value">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-fill-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-fill-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-fill-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-fill-return">#</a>

This method waits for [actionability](https://playwright.dev/python/docs/actionability) checks, focuses the element, fills it and triggers an `input` event after filling. Note that you can pass an empty string to clear the input field.

If the target element is not an `\<input>`, `\<textarea>` or `[contenteditable]` element, this method throws an error. However, if the element is inside the `\<label>` element that has an associated [control](https://developer.mozilla.org/en-US/docs/Web/API/HTMLLabelElement/control), the control will be filled instead.

To send fine-grained keyboard events, use [element_handle.type(text, **kwargs)](#element-handle-type).

## element_handle.focus()<a name="element-handle-focus">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-focus-return">#</a>

Calls [focus](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus) on the element.

## element_handle.get_attribute(name)<a name="element-handle-get-attribute">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Attribute name to get the value for.<a name="element-handle-get-attribute-option-name">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="element-handle-get-attribute-return">#</a>

Returns element attribute value.

## element_handle.hover(**kwargs)<a name="element-handle-hover">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-hover-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="element-handle-hover-option-modifiers">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="element-handle-hover-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-hover-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="element-handle-hover-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-hover-return">#</a>

This method hovers over the element by performing the following steps:

1. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
2. Scroll the element into view if needed.
3. Use [page.mouse](#page-mouse) to hover over the center of the element, or the specified `position`.
4. Wait for initiated navigations to either succeed or fail, unless `noWaitAfter` option is set.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## element_handle.inner_html()<a name="element-handle-inner-html">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="element-handle-inner-html-return">#</a>

Returns the `element.innerHTML`.

## element_handle.inner_text()<a name="element-handle-inner-text">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="element-handle-inner-text-return">#</a>

Returns the `element.innerText`.

## element_handle.input_value(**kwargs)<a name="element-handle-input-value">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-input-value-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="element-handle-input-value-return">#</a>

Returns `input.value` for `\<input>` or `\<textarea>` or `\<select>` element. Throws for non-input elements.

## element_handle.is_checked()<a name="element-handle-is-checked">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="element-handle-is-checked-return">#</a>

Returns whether the element is checked. Throws if the element is not a checkbox or radio input.

## element_handle.is_disabled()<a name="element-handle-is-disabled">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="element-handle-is-disabled-return">#</a>

Returns whether the element is disabled, the opposite of [enabled](https://playwright.dev/python/docs/actionability#enabled).

## element_handle.is_editable()<a name="element-handle-is-editable">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="element-handle-is-editable-return">#</a>

Returns whether the element is [editable](https://playwright.dev/python/docs/actionability#editable).

## element_handle.is_enabled()<a name="element-handle-is-enabled">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="element-handle-is-enabled-return">#</a>

Returns whether the element is [enabled](https://playwright.dev/python/docs/actionability#enabled).

## element_handle.is_hidden()<a name="element-handle-is-hidden">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="element-handle-is-hidden-return">#</a>

Returns whether the element is hidden, the opposite of [visible](https://playwright.dev/python/docs/actionability#visible).

## element_handle.is_visible()<a name="element-handle-is-visible">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="element-handle-is-visible-return">#</a>

Returns whether the element is [visible](https://playwright.dev/python/docs/actionability#visible).

## element_handle.owner_frame()<a name="element-handle-owner-frame">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Frame](#frame)><a name="element-handle-owner-frame-return">#</a>

Returns the frame containing the given element.

## element_handle.press(key, **kwargs)<a name="element-handle-press">#</a>

- `key` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the key to press or a character to generate, such as `ArrowLeft` or `a`.<a name="element-handle-press-option-key">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `keydown` and `keyup` in milliseconds. Defaults to 0.<a name="element-handle-press-option-delay">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-press-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-press-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-press-return">#</a>

Focuses the element, and then uses [keyboard.down(key)](#keyboard-down) and [keyboard.up(key)](#keyboard-up).

`key` can specify the intended [keyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key) value or a single character to generate the text for. A superset of the `key` values can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values). Examples of the keys are:

`F1` - `F12`, `Digit0`- `Digit9`, `KeyA`- `KeyZ`, `Backquote`, `Minus`, `Equal`, `Backslash`, `Backspace`, `Tab`, `Delete`, `Escape`, `ArrowDown`, `End`, `Enter`, `Home`, `Insert`, `PageDown`, `PageUp`, `ArrowRight`, `ArrowUp`, etc.

Following modification shortcuts are also supported: `Shift`, `Control`, `Alt`, `Meta`, `ShiftLeft`.

Holding down `Shift` will type the text that corresponds to the `key` in the upper case.

If `key` is a single character, it is case-sensitive, so the values `a` and `A` will generate different respective texts.

Shortcuts such as `key: "Control+o"` or `key: "Control+Shift+T"` are supported as well. When specified with the modifier, modifier is pressed and being held while the subsequent key is being pressed.

## element_handle.query_selector(selector)<a name="element-handle-query-selector">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="element-handle-query-selector-option-selector">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[ElementHandle](#elementhandle)><a name="element-handle-query-selector-return">#</a>

The method finds an element matching the specified selector in the `ElementHandle`'s subtree. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more details. If no elements match the selector, returns `null`.

## element_handle.query_selector_all(selector)<a name="element-handle-query-selector-all">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="element-handle-query-selector-all-option-selector">#</a>
- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[ElementHandle](#elementhandle)]><a name="element-handle-query-selector-all-return">#</a>

The method finds all elements matching the specified selector in the `ElementHandle`s subtree. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more details. If no elements match the selector, returns empty array.

## element_handle.screenshot(**kwargs)<a name="element-handle-screenshot">#</a>

- `animations` \<"disabled"> When set to `"disabled"`, stops CSS animations, CSS transitions and Web Animations. Animations get different treatment depending on their duration:<a name="element-handle-screenshot-option-animations">#</a>
    - finite animations are fast-forwarded to completion, so they'll fire `transitionend` event.
    - infinite animations are canceled to initial state, and then played over after the screenshot.
- `mask` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Locator](#locator)]> Specify locators that should be masked when the screenshot is taken. Masked elements will be overlayed with a pink box `#FF00FF` that completely covers its bounding box.<a name="element-handle-screenshot-option-mask">#</a>
- `omit_background` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Hides default white background and allows capturing screenshots with transparency. Not applicable to `jpeg` images. Defaults to `false`.<a name="element-handle-screenshot-option-omit-background">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> The file path to save the image to. The screenshot type will be inferred from file extension. If `path` is a relative path, then it is resolved relative to the current working directory. If no path is provided, the image won't be saved to the disk.<a name="element-handle-screenshot-option-path">#</a>
- `quality` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> The quality of the image, between 0-100. Not applicable to `png` images.<a name="element-handle-screenshot-option-quality">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-screenshot-option-timeout">#</a>
- `type` \<"png"|"jpeg"> Specify screenshot type, defaults to `png`.<a name="element-handle-screenshot-option-type">#</a>
- returns: \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)><a name="element-handle-screenshot-return">#</a>

Returns the buffer with the captured screenshot.

This method waits for the [actionability](https://playwright.dev/python/docs/actionability) checks, then scrolls element into view before taking a screenshot. If the element is detached from DOM, the method throws an error.

## element_handle.scroll_into_view_if_needed(**kwargs)<a name="element-handle-scroll-into-view-if-needed">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-scroll-into-view-if-needed-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-scroll-into-view-if-needed-return">#</a>

This method waits for [actionability](https://playwright.dev/python/docs/actionability) checks, then tries to scroll element into view, unless it is completely visible as defined by [IntersectionObserver](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)'s `ratio`.

Throws when `elementHandle` does not point to an element [connected](https://developer.mozilla.org/en-US/docs/Web/API/Node/isConnected) to a Document or a ShadowRoot.

## element_handle.select_option(**kwargs)<a name="element-handle-select-option">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-select-option-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-select-option-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-select-option-option-timeout">#</a>
- `element` \<[ElementHandle](#elementhandle)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[ElementHandle](#elementhandle)]> Option elements to select. Optional.<a name="element-handle-select-option-option-element">#</a>
- `index` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)]> Options to select by index. Optional.<a name="element-handle-select-option-option-index">#</a>
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Options to select by value. If the `\<select>` has the `multiple` attribute, all given options are selected, otherwise only the first option matching one of the passed options is selected. Optional.<a name="element-handle-select-option-option-value">#</a>
- `label` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Options to select by label. If the `\<select>` has the `multiple` attribute, all given options are selected, otherwise only the first option matching one of the passed options is selected. Optional.<a name="element-handle-select-option-option-label">#</a>
- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="element-handle-select-option-return">#</a>

This method waits for [actionability](https://playwright.dev/python/docs/actionability) checks, waits until all specified options are present in the `\<select>` element and selects these options.

If the target element is not a `\<select>` element, this method throws an error. However, if the element is inside the `\<label>` element that has an associated [control](https://developer.mozilla.org/en-US/docs/Web/API/HTMLLabelElement/control), the control will be used instead.

Returns the array of option values that have been successfully selected.

Triggers a `change` and `input` event once all the provided options have been selected.

- Sync

```python
# single selection matching the value
handle.select_option("blue")
# single selection matching both the label
handle.select_option(label="blue")
# multiple selection
handle.select_option(value=["red", "green", "blue"])
```

- Async

```python
# single selection matching the value
await handle.select_option("blue")
# single selection matching the label
await handle.select_option(label="blue")
# multiple selection
await handle.select_option(value=["red", "green", "blue"])
```

```python
# sync

# single selection matching the value
handle.select_option("blue")
# single selection matching both the value and the label
handle.select_option(label="blue")
# multiple selection
handle.select_option("red", "green", "blue")
# multiple selection for blue, red and second option
handle.select_option(value="blue", { index: 2 }, "red")
```

## element_handle.select_text(**kwargs)<a name="element-handle-select-text">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-select-text-option-force">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-select-text-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-select-text-return">#</a>

This method waits for [actionability](https://playwright.dev/python/docs/actionability) checks, then focuses the element and selects all its text content.

## element_handle.set_checked(checked, **kwargs)<a name="element-handle-set-checked">#</a>

- `checked` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to check or uncheck the checkbox.<a name="element-handle-set-checked-option-checked">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-set-checked-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-set-checked-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="element-handle-set-checked-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-set-checked-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="element-handle-set-checked-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-set-checked-return">#</a>

This method checks or unchecks an element by performing the following steps:

1. Ensure that element is a checkbox or a radio input. If not, this method throws.
2. If the element already has the right checked state, this method returns immediately.
3. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
4. Scroll the element into view if needed.
5. Use [page.mouse](#page-mouse) to click in the center of the element.
6. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
7. Ensure that the element is now checked or unchecked. If not, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## element_handle.set_input_files(files, **kwargs)<a name="element-handle-set-input-files">#</a>

- `files` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="element-handle-set-input-files-option-files">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-set-input-files-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-set-input-files-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-set-input-files-return">#</a>

This method expects `elementHandle` to point to an [input element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input).

Sets the value of the file input to these file paths or files. If some of the `filePaths` are relative paths, then they are resolved relative to the the current working directory. For empty array, clears the selected files.

## element_handle.tap(**kwargs)<a name="element-handle-tap">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-tap-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="element-handle-tap-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-tap-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="element-handle-tap-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-tap-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="element-handle-tap-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-tap-return">#</a>

This method taps the element by performing the following steps:

1. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
2. Scroll the element into view if needed.
3. Use [page.touchscreen](#page-touchscreen) to tap the center of the element, or the specified `position`.
4. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

> NOTE
>
> `elementHandle.tap()` requires that the `hasTouch` option of the browser context be set to true.

## element_handle.text_content()<a name="element-handle-text-content">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="element-handle-text-content-return">#</a>

Returns the `node.textContent`.

## element_handle.type(text, **kwargs)<a name="element-handle-type">#</a>

- `text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A text to type into a focused element.<a name="element-handle-type-option-text">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between key presses in milliseconds. Defaults to 0.<a name="element-handle-type-option-delay">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-type-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-type-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-type-return">#</a>

Focuses the element, and then sends a `keydown`, `keypress`/`input`, and `keyup` event for each character in the text.

To press a special key, like `Control` or `ArrowDown`, use [element_handle.press(key, **kwargs)](#element-handle-press).

- Sync

```python
element_handle.type("hello") # types instantly
element_handle.type("world", delay=100) # types slower, like a user
```

- Async

```python
await element_handle.type("hello") # types instantly
await element_handle.type("world", delay=100) # types slower, like a user
```

An example of typing into a text field and then submitting the form:

- Sync

```python
element_handle = page.query_selector("input")
element_handle.type("some text")
element_handle.press("Enter")
```

- Async

```python
element_handle = await page.query_selector("input")
await element_handle.type("some text")
await element_handle.press("Enter")
```

## element_handle.uncheck(**kwargs)<a name="element-handle-uncheck">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="element-handle-uncheck-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="element-handle-uncheck-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="element-handle-uncheck-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-uncheck-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="element-handle-uncheck-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-uncheck-return">#</a>

This method checks the element by performing the following steps:

1. Ensure that element is a checkbox or a radio input. If not, this method throws. If the element is already unchecked, this method returns immediately.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to click in the center of the element.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
6. Ensure that the element is now unchecked. If not, this method throws.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## element_handle.wait_for_element_state(state, **kwargs)<a name="element-handle-wait-for-element-state">#</a>

- `state` \<"visible"|"hidden"|"stable"|"enabled"|"disabled"|"editable"> A state to wait for, see below for more details.<a name="element-handle-wait-for-element-state-option-state">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-wait-for-element-state-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="element-handle-wait-for-element-state-return">#</a>

Returns when the element satisfies the `state`.

Depending on the `state` parameter, this method waits for one of the [actionability](https://playwright.dev/python/docs/actionability) checks to pass. This method throws when the element is detached while waiting, unless waiting for the `"hidden"` state.

- `"visible"` Wait until the element is [visible](https://playwright.dev/python/docs/actionability#visible).
- `"hidden"` Wait until the element is [not visible](https://playwright.dev/python/docs/actionability#visible) or [not attached](https://playwright.dev/python/docs/actionability#attached). Note that waiting for hidden does not throw when the element detaches.
- `"stable"` Wait until the element is both [visible](https://playwright.dev/python/docs/actionability#visible) and [stable](https://playwright.dev/python/docs/actionability#stable).
- `"enabled"` Wait until the element is [enabled](https://playwright.dev/python/docs/actionability#enabled).
- `"disabled"` Wait until the element is [not enabled](https://playwright.dev/python/docs/actionability#enabled).
- `"editable"` Wait until the element is [editable](https://playwright.dev/python/docs/actionability#editable).

If the element does not satisfy the condition for the `timeout` milliseconds, this method will throw.

## element_handle.wait_for_selector(selector, **kwargs)<a name="element-handle-wait-for-selector">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="element-handle-wait-for-selector-option-selector">#</a>
- `state` \<"attached"|"detached"|"visible"|"hidden"> Defaults to `'visible'`. Can be either:<a name="element-handle-wait-for-selector-option-state">#</a>
    - `'attached'` - wait for element to be present in DOM.
    - `'detached'` - wait for element to not be present in DOM.
    - `'visible'` - wait for element to have non-empty bounding box and no `visibility:hidden`. Note that element without any content or with `display:none` has an empty bounding box and is not considered visible.
    - `'hidden'` - wait for element to be either detached from DOM, or have an empty bounding box or `visibility:hidden`. This is opposite to the `'visible'` option.
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="element-handle-wait-for-selector-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="element-handle-wait-for-selector-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[ElementHandle](#elementhandle)><a name="element-handle-wait-for-selector-return">#</a>

Returns element specified by selector when it satisfies `state` option. Returns `null` if waiting for `hidden` or `detached`.

Wait for the `selector` relative to the element handle to satisfy `state` option (either appear/disappear from dom, or become visible/hidden). If at the moment of calling the method `selector` already satisfies the condition, the method will return immediately. If the selector doesn't satisfy the condition for the `timeout` milliseconds, the function will throw.

- Sync

```python
page.set_content("\<div>\<span>\</span>\</div>")
div = page.query_selector("div")
# waiting for the "span" selector relative to the div.
span = div.wait_for_selector("span", state="attached")
```

- Async

```python
await page.set_content("\<div>\<span>\</span>\</div>")
div = await page.query_selector("div")
# waiting for the "span" selector relative to the div.
span = await div.wait_for_selector("span", state="attached")
```

> NOTE
>
> This method does not work across navigations, use [page.wait_for_selector(selector, **kwargs)](#page-wait-for-selector) instead.





# Error

- extends: [Exception](https://docs.python.org/3/library/exceptions.html#Exception)

Error is raised whenever certain operations are terminated abnormally, e.g. browser closes while [page.evaluate(expression, **kwargs)](#page-evaluate) is running. All Playwright exceptions inherit from this class.

- [error.message](#error-message)
- [error.name](#error-name)
- [error.stack](#error-stack)

## error.message<a name="error-message">#</a>

- type: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

Message of the error.

## error.name<a name="error-name">#</a>

- type: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

Name of the error which got thrown inside the browser. Optional.

## error.stack<a name="error-stack">#</a>

- type: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)>

Stack of the error which got thrown inside the browser. Optional.



# FileChooser

[FileChooser](#filechooser) objects are dispatched by the page in the [page.on("filechooser")](#page-event-file-chooser) event.

- Sync

```python
with page.expect_file_chooser() as fc_info:
    page.click("upload")
file_chooser = fc_info.value
file_chooser.set_files("myfile.pdf")
```

- Async

```python
async with page.expect_file_chooser() as fc_info:
    await page.click("upload")
file_chooser = await fc_info.value
await file_chooser.set_files("myfile.pdf")
```

- [file_chooser.element](#file-chooser-element)
- [file_chooser.is_multiple()](#file-chooser-is-multiple)
- [file_chooser.page](#file-chooser-page)
- [file_chooser.set_files(files, **kwargs)](#file-chooser-set-files)

## file_chooser.element<a name="file-chooser-element">#</a>

- returns: \<[ElementHandle](#elementhandle)><a name="file-chooser-element-return">#</a>

Returns input element associated with this file chooser.

## file_chooser.is_multiple()<a name="file-chooser-is-multiple">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="file-chooser-is-multiple-return">#</a>

Returns whether this file chooser accepts multiple files.

## file_chooser.page<a name="file-chooser-page">#</a>

- returns: \<[Page](#page)><a name="file-chooser-page-return">#</a>

Returns page this file chooser belongs to.

## file_chooser.set_files(files, **kwargs)<a name="file-chooser-set-files">#</a>

- `files` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="file-chooser-set-files-option-files">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="file-chooser-set-files-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="file-chooser-set-files-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="file-chooser-set-files-return">#</a>

Sets the value of the file input this chooser is associated with. If some of the `filePaths` are relative paths, then they are resolved relative to the the current working directory. For empty array, clears the selected files.



# Frame

At every point of time, page exposes its current frame tree via the [page.main_frame](#page-main-frame) and [frame.child_frames](#frame-child-frames) methods.

[Frame](#frame) object's lifecycle is controlled by three events, dispatched on the page object:

- [page.on("frameattached")](#page-event-frame-attached) - fired when the frame gets attached to the page. A Frame can be attached to the page only once.
- [page.on("framenavigated")](#page-event-frame-navigated) - fired when the frame commits navigation to a different URL.
- [page.on("framedetached")](#page-event-frame-detached) - fired when the frame gets detached from the page. A Frame can be detached from the page only once.

An example of dumping frame tree:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    firefox = playwright.firefox
    browser = firefox.launch()
    page = browser.new_page()
    page.goto("https://www.theverge.com")
    dump_frame_tree(page.main_frame, "")
    browser.close()

def dump_frame_tree(frame, indent):
    print(indent + frame.name + '@' + frame.url)
    for child in frame.child_frames:
        dump_frame_tree(child, indent + "    ")

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    firefox = playwright.firefox
    browser = await firefox.launch()
    page = await browser.new_page()
    await page.goto("https://www.theverge.com")
    dump_frame_tree(page.main_frame, "")
    await browser.close()

def dump_frame_tree(frame, indent):
    print(indent + frame.name + '@' + frame.url)
    for child in frame.child_frames:
        dump_frame_tree(child, indent + "    ")

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

- [frame.add_script_tag(**kwargs)](#frame-add-script-tag)
- [frame.add_style_tag(**kwargs)](#frame-add-style-tag)
- [frame.check(selector, **kwargs)](#frame-check)
- [frame.child_frames](#frame-child-frames)
- [frame.click(selector, **kwargs)](#frame-click)
- [frame.content()](#frame-content)
- [frame.dblclick(selector, **kwargs)](#frame-dblclick)
- [frame.dispatch_event(selector, type, **kwargs)](#frame-dispatch-event)
- [frame.drag_and_drop(source, target, **kwargs)](#frame-drag-and-drop)
- [frame.eval_on_selector(selector, expression, **kwargs)](#frame-eval-on-selector)
- [frame.eval_on_selector_all(selector, expression, **kwargs)](#frame-eval-on-selector-all)
- [frame.evaluate(expression, **kwargs)](#frame-evaluate)
- [frame.evaluate_handle(expression, **kwargs)](#frame-evaluate-handle)
- [frame.expect_navigation(**kwargs)](#frame-wait-for-navigation)
- [frame.fill(selector, value, **kwargs)](#frame-fill)
- [frame.focus(selector, **kwargs)](#frame-focus)
- [frame.frame_element()](#frame-frame-element)
- [frame.frame_locator(selector)](#frame-frame-locator)
- [frame.get_attribute(selector, name, **kwargs)](#frame-get-attribute)
- [frame.goto(url, **kwargs)](#frame-goto)
- [frame.hover(selector, **kwargs)](#frame-hover)
- [frame.inner_html(selector, **kwargs)](#frame-inner-html)
- [frame.inner_text(selector, **kwargs)](#frame-inner-text)
- [frame.input_value(selector, **kwargs)](#frame-input-value)
- [frame.is_checked(selector, **kwargs)](#frame-is-checked)
- [frame.is_detached()](#frame-is-detached)
- [frame.is_disabled(selector, **kwargs)](#frame-is-disabled)
- [frame.is_editable(selector, **kwargs)](#frame-is-editable)
- [frame.is_enabled(selector, **kwargs)](#frame-is-enabled)
- [frame.is_hidden(selector, **kwargs)](#frame-is-hidden)
- [frame.is_visible(selector, **kwargs)](#frame-is-visible)
- [frame.locator(selector, **kwargs)](#frame-locator)
- [frame.name](#frame-name)
- [frame.page](#frame-page)
- [frame.parent_frame](#frame-parent-frame)
- [frame.press(selector, key, **kwargs)](#frame-press)
- [frame.query_selector(selector, **kwargs)](#frame-query-selector)
- [frame.query_selector_all(selector)](#frame-query-selector-all)
- [frame.select_option(selector, **kwargs)](#frame-select-option)
- [frame.set_checked(selector, checked, **kwargs)](#frame-set-checked)
- [frame.set_content(html, **kwargs)](#frame-set-content)
- [frame.set_input_files(selector, files, **kwargs)](#frame-set-input-files)
- [frame.tap(selector, **kwargs)](#frame-tap)
- [frame.text_content(selector, **kwargs)](#frame-text-content)
- [frame.title()](#frame-title)
- [frame.type(selector, text, **kwargs)](#frame-type)
- [frame.uncheck(selector, **kwargs)](#frame-uncheck)
- [frame.url](#frame-url)
- [frame.wait_for_function(expression, **kwargs)](#frame-wait-for-function)
- [frame.wait_for_load_state(**kwargs)](#frame-wait-for-load-state)
- [frame.wait_for_selector(selector, **kwargs)](#frame-wait-for-selector)
- [frame.wait_for_timeout(timeout)](#frame-wait-for-timeout)
- [frame.wait_for_url(url, **kwargs)](#frame-wait-for-url)

## frame.add_script_tag(**kwargs)<a name="frame-add-script-tag">#</a>

- `content` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Raw JavaScript content to be injected into frame.<a name="frame-add-script-tag-option-content">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to the JavaScript file to be injected into frame. If `path` is a relative path, then it is resolved relative to the current working directory.<a name="frame-add-script-tag-option-path">#</a>
- `type` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Script type. Use 'module' in order to load a Javascript ES6 module. See [script](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) for more details.<a name="frame-add-script-tag-option-type">#</a>
- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> URL of a script to be added.<a name="frame-add-script-tag-option-url">#</a>
- returns: \<[ElementHandle](#elementhandle)><a name="frame-add-script-tag-return">#</a>

Returns the added tag when the script's onload fires or when the script content was injected into frame.

Adds a `\<script>` tag into the page with the desired url or content.

## frame.add_style_tag(**kwargs)<a name="frame-add-style-tag">#</a>

- `content` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Raw CSS content to be injected into frame.<a name="frame-add-style-tag-option-content">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to the CSS file to be injected into frame. If `path` is a relative path, then it is resolved relative to the current working directory.<a name="frame-add-style-tag-option-path">#</a>
- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> URL of the `\<link>` tag.<a name="frame-add-style-tag-option-url">#</a>
- returns: \<[ElementHandle](#elementhandle)><a name="frame-add-style-tag-return">#</a>

Returns the added tag when the stylesheet's onload fires or when the CSS content was injected into frame.

Adds a `\<link rel="stylesheet">` tag into the page with the desired url or a `\<style type="text/css">` tag with the content.

## frame.check(selector, **kwargs)<a name="frame-check">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-check-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-check-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-check-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="frame-check-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-check-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-check-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="frame-check-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-check-return">#</a>

This method checks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Ensure that matched element is a checkbox or a radio input. If not, this method throws. If the element is already checked, this method returns immediately.
3. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
4. Scroll the element into view if needed.
5. Use [page.mouse](#page-mouse) to click in the center of the element.
6. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
7. Ensure that the element is now checked. If not, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## frame.child_frames<a name="frame-child-frames">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Frame](#frame)]><a name="frame-child-frames-return">#</a>

## frame.click(selector, **kwargs)<a name="frame-click">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-click-option-selector">#</a>
- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="frame-click-option-button">#</a>
- `click_count` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> defaults to 1. See [UIEvent.detail](https://developer.mozilla.org/en-US/docs/Web/API/UIEvent/detail).<a name="frame-click-option-click-count">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="frame-click-option-delay">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-click-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="frame-click-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-click-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="frame-click-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-click-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-click-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="frame-click-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-click-return">#</a>

This method clicks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to click in the center of the element, or the specified `position`.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## frame.content()<a name="frame-content">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-content-return">#</a>

Gets the full HTML contents of the frame, including the doctype.

## frame.dblclick(selector, **kwargs)<a name="frame-dblclick">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-dblclick-option-selector">#</a>
- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="frame-dblclick-option-button">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="frame-dblclick-option-delay">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-dblclick-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="frame-dblclick-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-dblclick-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="frame-dblclick-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-dblclick-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-dblclick-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="frame-dblclick-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-dblclick-return">#</a>

This method double clicks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to double click in the center of the element, or the specified `position`.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set. Note that if the first click of the `dblclick()` triggers a navigation event, this method will throw.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

> NOTE
>
> `frame.dblclick()` dispatches two `click` events and a single `dblclick` event.

## frame.dispatch_event(selector, type, **kwargs)<a name="frame-dispatch-event">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-dispatch-event-option-selector">#</a>
- `type` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> DOM event type: `"click"`, `"dragstart"`, etc.<a name="frame-dispatch-event-option-type">#</a>
- `event_init` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional event-specific initialization properties.<a name="frame-dispatch-event-option-event-init">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-dispatch-event-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-dispatch-event-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-dispatch-event-return">#</a>

The snippet below dispatches the `click` event on the element. Regardless of the visibility state of the element, `click` is dispatched. This is equivalent to calling [element.click()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/click).

- Sync

```python
frame.dispatch_event("button#submit", "click")
```

- Async

```python
await frame.dispatch_event("button#submit", "click")
```

Under the hood, it creates an instance of an event based on the given `type`, initializes it with `event_init` properties and dispatches it on the element. Events are `composed`, `cancelable` and bubble by default.

Since `event_init` is event-specific, please refer to the events documentation for the lists of initial properties:

- [DragEvent](https://developer.mozilla.org/en-US/docs/Web/API/DragEvent/DragEvent)
- [FocusEvent](https://developer.mozilla.org/en-US/docs/Web/API/FocusEvent/FocusEvent)
- [KeyboardEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/KeyboardEvent)
- [MouseEvent](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/MouseEvent)
- [PointerEvent](https://developer.mozilla.org/en-US/docs/Web/API/PointerEvent/PointerEvent)
- [TouchEvent](https://developer.mozilla.org/en-US/docs/Web/API/TouchEvent/TouchEvent)
- [Event](https://developer.mozilla.org/en-US/docs/Web/API/Event/Event)

You can also specify `JSHandle` as the property value if you want live objects to be passed into the event:

- Sync

```python
# note you can only create data_transfer in chromium and firefox
data_transfer = frame.evaluate_handle("new DataTransfer()")
frame.dispatch_event("#source", "dragstart", { "dataTransfer": data_transfer })
```

- Async

```python
# note you can only create data_transfer in chromium and firefox
data_transfer = await frame.evaluate_handle("new DataTransfer()")
await frame.dispatch_event("#source", "dragstart", { "dataTransfer": data_transfer })
```

## frame.drag_and_drop(source, target, **kwargs)<a name="frame-drag-and-drop">#</a>

- `source` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-drag-and-drop-option-source">#</a>
- `target` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-drag-and-drop-option-target">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-drag-and-drop-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-drag-and-drop-option-no-wait-after">#</a>
- `source_position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Clicks on the source element at this point relative to the top-left corner of the element's padding box. If not specified, some visible point of the element is used.<a name="frame-drag-and-drop-option-source-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-drag-and-drop-option-strict">#</a>
- `target_position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Drops on the target element at this point relative to the top-left corner of the element's padding box. If not specified, some visible point of the element is used.<a name="frame-drag-and-drop-option-target-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-drag-and-drop-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="frame-drag-and-drop-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-drag-and-drop-return">#</a>

## frame.eval_on_selector(selector, expression, **kwargs)<a name="frame-eval-on-selector">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-eval-on-selector-option-selector">#</a>
- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="frame-eval-on-selector-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="frame-eval-on-selector-option-arg">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-eval-on-selector-option-strict">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="frame-eval-on-selector-return">#</a>

Returns the return value of `expression`.

> CAUTION
>
> This method does not wait for the element to pass actionability checks and therefore can lead to the flaky tests. Use [locator.evaluate(expression, **kwargs)](#locator-evaluate), other [Locator](#locator) helper methods or web-first assertions instead.

The method finds an element matching the specified selector within the frame and passes it as a first argument to `expression`. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more details. If no elements match the selector, the method throws an error.

If `expression` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [frame.eval_on_selector(selector, expression, **kwargs)](#frame-eval-on-selector) would wait for the promise to resolve and return its value.

Examples:

- Sync

```python
search_value = frame.eval_on_selector("#search", "el => el.value")
preload_href = frame.eval_on_selector("link[rel=preload]", "el => el.href")
html = frame.eval_on_selector(".main-container", "(e, suffix) => e.outerHTML + suffix", "hello")
```

- Async

```python
search_value = await frame.eval_on_selector("#search", "el => el.value")
preload_href = await frame.eval_on_selector("link[rel=preload]", "el => el.href")
html = await frame.eval_on_selector(".main-container", "(e, suffix) => e.outerHTML + suffix", "hello")
```

## frame.eval_on_selector_all(selector, expression, **kwargs)<a name="frame-eval-on-selector-all">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-eval-on-selector-all-option-selector">#</a>
- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="frame-eval-on-selector-all-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="frame-eval-on-selector-all-option-arg">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="frame-eval-on-selector-all-return">#</a>

Returns the return value of `expression`.

> NOTE
>
> In most cases, [locator.evaluate_all(expression, **kwargs)](#locator-evaluate-all), other [Locator](#locator) helper methods and web-first assertions do a better job.

The method finds all elements matching the specified selector within the frame and passes an array of matched elements as a first argument to `expression`. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more details.

If `expression` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [frame.eval_on_selector_all(selector, expression, **kwargs)](#frame-eval-on-selector-all) would wait for the promise to resolve and return its value.

Examples:

- Sync

```python
divs_counts = frame.eval_on_selector_all("div", "(divs, min) => divs.length >= min", 10)
```

- Async

```python
divs_counts = await frame.eval_on_selector_all("div", "(divs, min) => divs.length >= min", 10)
```

## frame.evaluate(expression, **kwargs)<a name="frame-evaluate">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="frame-evaluate-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="frame-evaluate-option-arg">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="frame-evaluate-return">#</a>

Returns the return value of `expression`.

If the function passed to the [frame.evaluate(expression, **kwargs)](#frame-evaluate) returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [frame.evaluate(expression, **kwargs)](#frame-evaluate) would wait for the promise to resolve and return its value.

If the function passed to the [frame.evaluate(expression, **kwargs)](#frame-evaluate) returns a non-[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description) value, then [frame.evaluate(expression, **kwargs)](#frame-evaluate) returns `undefined`. Playwright also supports transferring some additional values that are not serializable by `JSON`: `-0`, `NaN`, `Infinity`, `-Infinity`.

- Sync

```python
result = frame.evaluate("([x, y]) => Promise.resolve(x * y)", [7, 8])
print(result) # prints "56"
```

- Async

```python
result = await frame.evaluate("([x, y]) => Promise.resolve(x * y)", [7, 8])
print(result) # prints "56"
```

A string can also be passed in instead of a function.

- Sync

```python
print(frame.evaluate("1 + 2")) # prints "3"
x = 10
print(frame.evaluate(f"1 + {x}")) # prints "11"
```

- Async

```python
print(await frame.evaluate("1 + 2")) # prints "3"
x = 10
print(await frame.evaluate(f"1 + {x}")) # prints "11"
```

[ElementHandle](#elementhandle) instances can be passed as an argument to the [frame.evaluate(expression, **kwargs)](#frame-evaluate):

- Sync

```python
body_handle = frame.evaluate("document.body")
html = frame.evaluate("([body, suffix]) => body.innerHTML + suffix", [body_handle, "hello"])
body_handle.dispose()
```

- Async

```python
body_handle = await frame.evaluate("document.body")
html = await frame.evaluate("([body, suffix]) => body.innerHTML + suffix", [body_handle, "hello"])
await body_handle.dispose()
```

## frame.evaluate_handle(expression, **kwargs)<a name="frame-evaluate-handle">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="frame-evaluate-handle-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="frame-evaluate-handle-option-arg">#</a>
- returns: \<[JSHandle](#jshandle)><a name="frame-evaluate-handle-return">#</a>

Returns the return value of `expression` as a [JSHandle](#jshandle).

The only difference between [frame.evaluate(expression, **kwargs)](#frame-evaluate) and [frame.evaluate_handle(expression, **kwargs)](#frame-evaluate-handle) is that [frame.evaluate_handle(expression, **kwargs)](#frame-evaluate-handle) returns [JSHandle](#jshandle).

If the function, passed to the [frame.evaluate_handle(expression, **kwargs)](#frame-evaluate-handle), returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [frame.evaluate_handle(expression, **kwargs)](#frame-evaluate-handle) would wait for the promise to resolve and return its value.

- Sync

```python
a_window_handle = frame.evaluate_handle("Promise.resolve(window)")
a_window_handle # handle for the window object.
```

- Async

```python
a_window_handle = await frame.evaluate_handle("Promise.resolve(window)")
a_window_handle # handle for the window object.
```

A string can also be passed in instead of a function.

- Sync

```python
a_handle = page.evaluate_handle("document") # handle for the "document"
```

- Async

```python
a_handle = await page.evaluate_handle("document") # handle for the "document"
```

[JSHandle](#jshandle) instances can be passed as an argument to the [frame.evaluate_handle(expression, **kwargs)](#frame-evaluate-handle):

- Sync

```python
a_handle = page.evaluate_handle("document.body")
result_handle = page.evaluate_handle("body => body.innerHTML", a_handle)
print(result_handle.json_value())
result_handle.dispose()
```

- Async

```python
a_handle = await page.evaluate_handle("document.body")
result_handle = await page.evaluate_handle("body => body.innerHTML", a_handle)
print(await result_handle.json_value())
await result_handle.dispose()
```

## frame.expect_navigation(**kwargs)<a name="frame-wait-for-navigation">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-wait-for-navigation-option-timeout">#</a>
- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[URL](https://en.wikipedia.org/wiki/URL)]:[bool](https://docs.python.org/3/library/stdtypes.html)> A glob pattern, regex pattern or predicate receiving [URL](https://en.wikipedia.org/wiki/URL) to match while waiting for the navigation. Note that if the parameter is a string without wilcard characters, the method will wait for navigation to URL that is exactly equal to the string.<a name="frame-wait-for-navigation-option-url">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="frame-wait-for-navigation-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[Response](#response)]><a name="frame-wait-for-navigation-return">#</a>

Waits for the frame navigation and returns the main resource response. In case of multiple redirects, the navigation will resolve with the response of the last redirect. In case of navigation to a different anchor or navigation due to History API usage, the navigation will resolve with `null`.

This method waits for the frame to navigate to a new URL. It is useful for when you run code which will indirectly cause the frame to navigate. Consider this example:

- Sync

```python
with frame.expect_navigation():
    frame.click("a.delayed-navigation") # clicking the link will indirectly cause a navigation
# Resolves after navigation has finished
```

- Async

```python
async with frame.expect_navigation():
    await frame.click("a.delayed-navigation") # clicking the link will indirectly cause a navigation
# Resolves after navigation has finished
```

> NOTE
>
> Usage of the [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) to change the URL is considered a navigation.

## frame.fill(selector, value, **kwargs)<a name="frame-fill">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-fill-option-selector">#</a>
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Value to fill for the `\<input>`, `\<textarea>` or `[contenteditable]` element.<a name="frame-fill-option-value">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-fill-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-fill-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-fill-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-fill-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-fill-return">#</a>

This method waits for an element matching `selector`, waits for [actionability](https://playwright.dev/python/docs/actionability) checks, focuses the element, fills it and triggers an `input` event after filling. Note that you can pass an empty string to clear the input field.

If the target element is not an `\<input>`, `\<textarea>` or `[contenteditable]` element, this method throws an error. However, if the element is inside the `\<label>` element that has an associated [control](https://developer.mozilla.org/en-US/docs/Web/API/HTMLLabelElement/control), the control will be filled instead.

To send fine-grained keyboard events, use [frame.type(selector, text, **kwargs)](#frame-type).

## frame.focus(selector, **kwargs)<a name="frame-focus">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-focus-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-focus-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-focus-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-focus-return">#</a>

This method fetches an element with `selector` and focuses it. If there's no element matching `selector`, the method waits until a matching element appears in the DOM.

## frame.frame_element()<a name="frame-frame-element">#</a>

- returns: \<[ElementHandle](#elementhandle)><a name="frame-frame-element-return">#</a>

Returns the `frame` or `iframe` element handle which corresponds to this frame.

This is an inverse of [element_handle.content_frame()](#element-handle-content-frame). Note that returned handle actually belongs to the parent frame.

This method throws an error if the frame has been detached before `frameElement()` returns.

- Sync

```python
frame_element = frame.frame_element()
content_frame = frame_element.content_frame()
assert frame == content_frame
```

- Async

```python
frame_element = await frame.frame_element()
content_frame = await frame_element.content_frame()
assert frame == content_frame
```

## frame.frame_locator(selector)<a name="frame-frame-locator">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to use when resolving DOM element. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-frame-locator-option-selector">#</a>
- returns: \<[FrameLocator](#framelocator)><a name="frame-frame-locator-return">#</a>

When working with iframes, you can create a frame locator that will enter the iframe and allow selecting elements in that iframe. Following snippet locates element with text "Submit" in the iframe with id `my-frame`, like `\<iframe id="my-frame">`:

- Sync

```python
locator = frame.frame_locator("#my-iframe").locator("text=Submit")
locator.click()
```

- Async

```python
locator = frame.frame_locator("#my-iframe").locator("text=Submit")
await locator.click()
```

## frame.get_attribute(selector, name, **kwargs)<a name="frame-get-attribute">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-get-attribute-option-selector">#</a>
- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Attribute name to get the value for.<a name="frame-get-attribute-option-name">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-get-attribute-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-get-attribute-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-get-attribute-return">#</a>

Returns element attribute value.

## frame.goto(url, **kwargs)<a name="frame-goto">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> URL to navigate frame to. The url should include scheme, e.g. `https://`.<a name="frame-goto-option-url">#</a>
- `referer` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Referer header value. If provided it will take preference over the referer header value set by [page.set_extra_http_headers(headers)](#page-set-extra-http-headers).<a name="frame-goto-option-referer">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-goto-option-timeout">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="frame-goto-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Response](#response)><a name="frame-goto-return">#</a>

Returns the main resource response. In case of multiple redirects, the navigation will resolve with the response of the last redirect.

The method will throw an error if:

- there's an SSL error (e.g. in case of self-signed certificates).
- target URL is invalid.
- the `timeout` is exceeded during navigation.
- the remote server does not respond or is unreachable.
- the main resource failed to load.

The method will not throw an error when any valid HTTP status code is returned by the remote server, including 404 "Not Found" and 500 "Internal Server Error". The status code for such responses can be retrieved by calling [response.status](#response-status).

> NOTE
>
> The method either throws an error or returns a main resource response. The only exceptions are navigation to `about:blank` or navigation to the same URL with a different hash, which would succeed and return `null`.

> NOTE
>
> Headless mode doesn't support navigation to a PDF document. See the [upstream issue](https://bugs.chromium.org/p/chromium/issues/detail?id=761295).

## frame.hover(selector, **kwargs)<a name="frame-hover">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-hover-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-hover-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="frame-hover-option-modifiers">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="frame-hover-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-hover-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-hover-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="frame-hover-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-hover-return">#</a>

This method hovers over an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to hover over the center of the element, or the specified `position`.
5. Wait for initiated navigations to either succeed or fail, unless `noWaitAfter` option is set.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## frame.inner_html(selector, **kwargs)<a name="frame-inner-html">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-inner-html-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-inner-html-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-inner-html-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-inner-html-return">#</a>

Returns `element.innerHTML`.

## frame.inner_text(selector, **kwargs)<a name="frame-inner-text">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-inner-text-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-inner-text-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-inner-text-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-inner-text-return">#</a>

Returns `element.innerText`.

## frame.input_value(selector, **kwargs)<a name="frame-input-value">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-input-value-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-input-value-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-input-value-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-input-value-return">#</a>

Returns `input.value` for the selected `\<input>` or `\<textarea>` or `\<select>` element. Throws for non-input elements.

## frame.is_checked(selector, **kwargs)<a name="frame-is-checked">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-is-checked-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-is-checked-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-is-checked-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="frame-is-checked-return">#</a>

Returns whether the element is checked. Throws if the element is not a checkbox or radio input.

## frame.is_detached()<a name="frame-is-detached">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="frame-is-detached-return">#</a>

Returns `true` if the frame has been detached, or `false` otherwise.

## frame.is_disabled(selector, **kwargs)<a name="frame-is-disabled">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-is-disabled-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-is-disabled-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-is-disabled-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="frame-is-disabled-return">#</a>

Returns whether the element is disabled, the opposite of [enabled](https://playwright.dev/python/docs/actionability#enabled).

## frame.is_editable(selector, **kwargs)<a name="frame-is-editable">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-is-editable-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-is-editable-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-is-editable-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="frame-is-editable-return">#</a>

Returns whether the element is [editable](https://playwright.dev/python/docs/actionability#editable).

## frame.is_enabled(selector, **kwargs)<a name="frame-is-enabled">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-is-enabled-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-is-enabled-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-is-enabled-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="frame-is-enabled-return">#</a>

Returns whether the element is [enabled](https://playwright.dev/python/docs/actionability#enabled).

## frame.is_hidden(selector, **kwargs)<a name="frame-is-hidden">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-is-hidden-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-is-hidden-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> **DEPRECATED** This option is ignored. [frame.is_hidden(selector, **kwargs)](#frame-is-hidden) does not wait for the element to become hidden and returns immediately.<a name="frame-is-hidden-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="frame-is-hidden-return">#</a>

Returns whether the element is hidden, the opposite of [visible](https://playwright.dev/python/docs/actionability#visible). `selector` that does not match any elements is considered hidden.

## frame.is_visible(selector, **kwargs)<a name="frame-is-visible">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-is-visible-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-is-visible-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> **DEPRECATED** This option is ignored. [frame.is_visible(selector, **kwargs)](#frame-is-visible) does not wait for the element to become visible and returns immediately.<a name="frame-is-visible-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="frame-is-visible-return">#</a>

Returns whether the element is [visible](https://playwright.dev/python/docs/actionability#visible). `selector` that does not match any elements is considered not visible.

## frame.locator(selector, **kwargs)<a name="frame-locator">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to use when resolving DOM element. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-locator-option-selector">#</a>

- `has` \<[Locator](#locator)> Matches elements containing an element that matches an inner locator. Inner locator is queried against the outer one. For example, `article` that has `text=Playwright` matches `\<article>\<div>Playwright\</div>\</article>`.<a name="frame-locator-option-has">#</a>

    Note that outer and inner locators must belong to the same frame. Inner locator must not contain [FrameLocator](#framelocator)s.

- `has_text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Matches elements containing specified text somewhere inside, possibly in a child or a descendant element. For example, `"Playwright"` matches `\<article>\<div>Playwright\</div>\</article>`.<a name="frame-locator-option-has-text">#</a>

- returns: \<[Locator](#locator)><a name="frame-locator-return">#</a>

The method returns an element locator that can be used to perform actions in the frame. Locator is resolved to the element immediately before performing an action, so a series of actions on the same locator can in fact be performed on different DOM elements. That would happen if the DOM structure between those actions has changed.

## frame.name<a name="frame-name">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-name-return">#</a>

Returns frame's name attribute as specified in the tag.

If the name is empty, returns the id attribute instead.

> NOTE
>
> This value is calculated once when the frame is created, and will not update if the attribute is changed later.

## frame.page<a name="frame-page">#</a>

- returns: \<[Page](#page)><a name="frame-page-return">#</a>

Returns the page containing this frame.

## frame.parent_frame<a name="frame-parent-frame">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Frame](#frame)><a name="frame-parent-frame-return">#</a>

Parent frame, if any. Detached frames and main frames return `null`.

## frame.press(selector, key, **kwargs)<a name="frame-press">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-press-option-selector">#</a>
- `key` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the key to press or a character to generate, such as `ArrowLeft` or `a`.<a name="frame-press-option-key">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `keydown` and `keyup` in milliseconds. Defaults to 0.<a name="frame-press-option-delay">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-press-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-press-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-press-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-press-return">#</a>

`key` can specify the intended [keyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key) value or a single character to generate the text for. A superset of the `key` values can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values). Examples of the keys are:

`F1` - `F12`, `Digit0`- `Digit9`, `KeyA`- `KeyZ`, `Backquote`, `Minus`, `Equal`, `Backslash`, `Backspace`, `Tab`, `Delete`, `Escape`, `ArrowDown`, `End`, `Enter`, `Home`, `Insert`, `PageDown`, `PageUp`, `ArrowRight`, `ArrowUp`, etc.

Following modification shortcuts are also supported: `Shift`, `Control`, `Alt`, `Meta`, `ShiftLeft`.

Holding down `Shift` will type the text that corresponds to the `key` in the upper case.

If `key` is a single character, it is case-sensitive, so the values `a` and `A` will generate different respective texts.

Shortcuts such as `key: "Control+o"` or `key: "Control+Shift+T"` are supported as well. When specified with the modifier, modifier is pressed and being held while the subsequent key is being pressed.

## frame.query_selector(selector, **kwargs)<a name="frame-query-selector">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-query-selector-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-query-selector-option-strict">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[ElementHandle](#elementhandle)><a name="frame-query-selector-return">#</a>

Returns the ElementHandle pointing to the frame element.

> CAUTION
>
> The use of [ElementHandle](#elementhandle) is discouraged, use [Locator](#locator) objects and web-first assertions instead.

The method finds an element matching the specified selector within the frame. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more details. If no elements match the selector, returns `null`.

## frame.query_selector_all(selector)<a name="frame-query-selector-all">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-query-selector-all-option-selector">#</a>
- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[ElementHandle](#elementhandle)]><a name="frame-query-selector-all-return">#</a>

Returns the ElementHandles pointing to the frame elements.

> CAUTION
>
> The use of [ElementHandle](#elementhandle) is discouraged, use [Locator](#locator) objects instead.

The method finds all elements matching the specified selector within the frame. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more details. If no elements match the selector, returns empty array.

## frame.select_option(selector, **kwargs)<a name="frame-select-option">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-select-option-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-select-option-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-select-option-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-select-option-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-select-option-option-timeout">#</a>
- `element` \<[ElementHandle](#elementhandle)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[ElementHandle](#elementhandle)]> Option elements to select. Optional.<a name="frame-select-option-option-element">#</a>
- `index` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)]> Options to select by index. Optional.<a name="frame-select-option-option-index">#</a>
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Options to select by value. If the `\<select>` has the `multiple` attribute, all given options are selected, otherwise only the first option matching one of the passed options is selected. Optional.<a name="frame-select-option-option-value">#</a>
- `label` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Options to select by label. If the `\<select>` has the `multiple` attribute, all given options are selected, otherwise only the first option matching one of the passed options is selected. Optional.<a name="frame-select-option-option-label">#</a>
- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="frame-select-option-return">#</a>

This method waits for an element matching `selector`, waits for [actionability](https://playwright.dev/python/docs/actionability) checks, waits until all specified options are present in the `\<select>` element and selects these options.

If the target element is not a `\<select>` element, this method throws an error. However, if the element is inside the `\<label>` element that has an associated [control](https://developer.mozilla.org/en-US/docs/Web/API/HTMLLabelElement/control), the control will be used instead.

Returns the array of option values that have been successfully selected.

Triggers a `change` and `input` event once all the provided options have been selected.

- Sync

```python
# single selection matching the value
frame.select_option("select#colors", "blue")
# single selection matching both the label
frame.select_option("select#colors", label="blue")
# multiple selection
frame.select_option("select#colors", value=["red", "green", "blue"])
```

- Async

```python
# single selection matching the value
await frame.select_option("select#colors", "blue")
# single selection matching the label
await frame.select_option("select#colors", label="blue")
# multiple selection
await frame.select_option("select#colors", value=["red", "green", "blue"])
```

## frame.set_checked(selector, checked, **kwargs)<a name="frame-set-checked">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-set-checked-option-selector">#</a>
- `checked` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to check or uncheck the checkbox.<a name="frame-set-checked-option-checked">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-set-checked-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-set-checked-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="frame-set-checked-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-set-checked-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-set-checked-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="frame-set-checked-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-set-checked-return">#</a>

This method checks or unchecks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Ensure that matched element is a checkbox or a radio input. If not, this method throws.
3. If the element already has the right checked state, this method returns immediately.
4. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
5. Scroll the element into view if needed.
6. Use [page.mouse](#page-mouse) to click in the center of the element.
7. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
8. Ensure that the element is now checked or unchecked. If not, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## frame.set_content(html, **kwargs)<a name="frame-set-content">#</a>

- `html` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> HTML markup to assign to the page.<a name="frame-set-content-option-html">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-set-content-option-timeout">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="frame-set-content-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-set-content-return">#</a>

## frame.set_input_files(selector, files, **kwargs)<a name="frame-set-input-files">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-set-input-files-option-selector">#</a>
- `files` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="frame-set-input-files-option-files">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-set-input-files-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-set-input-files-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-set-input-files-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-set-input-files-return">#</a>

This method expects `selector` to point to an [input element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input).

Sets the value of the file input to these file paths or files. If some of the `filePaths` are relative paths, then they are resolved relative to the the current working directory. For empty array, clears the selected files.

## frame.tap(selector, **kwargs)<a name="frame-tap">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-tap-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-tap-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="frame-tap-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-tap-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="frame-tap-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-tap-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-tap-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="frame-tap-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-tap-return">#</a>

This method taps an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
3. Scroll the element into view if needed.
4. Use [page.touchscreen](#page-touchscreen) to tap the center of the element, or the specified `position`.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

> NOTE
>
> `frame.tap()` requires that the `hasTouch` option of the browser context be set to true.

## frame.text_content(selector, **kwargs)<a name="frame-text-content">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-text-content-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-text-content-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-text-content-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-text-content-return">#</a>

Returns `element.textContent`.

## frame.title()<a name="frame-title">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-title-return">#</a>

Returns the page title.

## frame.type(selector, text, **kwargs)<a name="frame-type">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-type-option-selector">#</a>
- `text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A text to type into a focused element.<a name="frame-type-option-text">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between key presses in milliseconds. Defaults to 0.<a name="frame-type-option-delay">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-type-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-type-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-type-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-type-return">#</a>

Sends a `keydown`, `keypress`/`input`, and `keyup` event for each character in the text. `frame.type` can be used to send fine-grained keyboard events. To fill values in form fields, use [frame.fill(selector, value, **kwargs)](#frame-fill).

To press a special key, like `Control` or `ArrowDown`, use [keyboard.press(key, **kwargs)](#keyboard-press).

- Sync

```python
frame.type("#mytextarea", "hello") # types instantly
frame.type("#mytextarea", "world", delay=100) # types slower, like a user
```

- Async

```python
await frame.type("#mytextarea", "hello") # types instantly
await frame.type("#mytextarea", "world", delay=100) # types slower, like a user
```

## frame.uncheck(selector, **kwargs)<a name="frame-uncheck">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-uncheck-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="frame-uncheck-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="frame-uncheck-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="frame-uncheck-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-uncheck-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-uncheck-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="frame-uncheck-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-uncheck-return">#</a>

This method checks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Ensure that matched element is a checkbox or a radio input. If not, this method throws. If the element is already unchecked, this method returns immediately.
3. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
4. Scroll the element into view if needed.
5. Use [page.mouse](#page-mouse) to click in the center of the element.
6. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
7. Ensure that the element is now unchecked. If not, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## frame.url<a name="frame-url">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="frame-url-return">#</a>

Returns frame's url.

## frame.wait_for_function(expression, **kwargs)<a name="frame-wait-for-function">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="frame-wait-for-function-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="frame-wait-for-function-option-arg">#</a>
- `polling` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|"raf"> If `polling` is `'raf'`, then `expression` is constantly executed in `requestAnimationFrame` callback. If `polling` is a number, then it is treated as an interval in milliseconds at which the function would be executed. Defaults to `raf`.<a name="frame-wait-for-function-option-polling">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="frame-wait-for-function-option-timeout">#</a>
- returns: \<[JSHandle](#jshandle)><a name="frame-wait-for-function-return">#</a>

Returns when the `expression` returns a truthy value, returns that value.

The [frame.wait_for_function(expression, **kwargs)](#frame-wait-for-function) can be used to observe viewport size change:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    webkit = playwright.webkit
    browser = webkit.launch()
    page = browser.new_page()
    page.evaluate("window.x = 0; setTimeout(() => { window.x = 100 }, 1000);")
    page.main_frame.wait_for_function("() => window.x > 0")
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    webkit = playwright.webkit
    browser = await webkit.launch()
    page = await browser.new_page()
    await page.evaluate("window.x = 0; setTimeout(() => { window.x = 100 }, 1000);")
    await page.main_frame.wait_for_function("() => window.x > 0")
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

To pass an argument to the predicate of `frame.waitForFunction` function:

- Sync

```python
selector = ".foo"
frame.wait_for_function("selector => !!document.querySelector(selector)", selector)
```

- Async

```python
selector = ".foo"
await frame.wait_for_function("selector => !!document.querySelector(selector)", selector)
```

## frame.wait_for_load_state(**kwargs)<a name="frame-wait-for-load-state">#</a>

- `state` \<"load"|"domcontentloaded"|"networkidle"> Optional load state to wait for, defaults to `load`. If the state has been already reached while loading current document, the method resolves immediately. Can be one of:<a name="frame-wait-for-load-state-option-state">#</a>
    - `'load'` - wait for the `load` event to be fired.
    - `'domcontentloaded'` - wait for the `DOMContentLoaded` event to be fired.
    - `'networkidle'` - wait until there are no network connections for at least `500` ms.
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-wait-for-load-state-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-wait-for-load-state-return">#</a>

Waits for the required load state to be reached.

This returns when the frame reaches a required load state, `load` by default. The navigation must have been committed when this method is called. If current document has already reached the required state, resolves immediately.

- Sync

```python
frame.click("button") # click triggers navigation.
frame.wait_for_load_state() # the promise resolves after "load" event.
```

- Async

```python
await frame.click("button") # click triggers navigation.
await frame.wait_for_load_state() # the promise resolves after "load" event.
```

## frame.wait_for_selector(selector, **kwargs)<a name="frame-wait-for-selector">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-wait-for-selector-option-selector">#</a>
- `state` \<"attached"|"detached"|"visible"|"hidden"> Defaults to `'visible'`. Can be either:<a name="frame-wait-for-selector-option-state">#</a>
    - `'attached'` - wait for element to be present in DOM.
    - `'detached'` - wait for element to not be present in DOM.
    - `'visible'` - wait for element to have non-empty bounding box and no `visibility:hidden`. Note that element without any content or with `display:none` has an empty bounding box and is not considered visible.
    - `'hidden'` - wait for element to be either detached from DOM, or have an empty bounding box or `visibility:hidden`. This is opposite to the `'visible'` option.
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="frame-wait-for-selector-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-wait-for-selector-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[ElementHandle](#elementhandle)><a name="frame-wait-for-selector-return">#</a>

Returns when element specified by selector satisfies `state` option. Returns `null` if waiting for `hidden` or `detached`.

> NOTE
>
> Playwright automatically waits for element to be ready before performing an action. Using [Locator](#locator) objects and web-first assertions make the code wait-for-selector-free.

Wait for the `selector` to satisfy `state` option (either appear/disappear from dom, or become visible/hidden). If at the moment of calling the method `selector` already satisfies the condition, the method will return immediately. If the selector doesn't satisfy the condition for the `timeout` milliseconds, the function will throw.

This method works across navigations:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    chromium = playwright.chromium
    browser = chromium.launch()
    page = browser.new_page()
    for current_url in ["https://google.com", "https://bbc.com"]:
        page.goto(current_url, wait_until="domcontentloaded")
        element = page.main_frame.wait_for_selector("img")
        print("Loaded image: " + str(element.get_attribute("src")))
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    chromium = playwright.chromium
    browser = await chromium.launch()
    page = await browser.new_page()
    for current_url in ["https://google.com", "https://bbc.com"]:
        await page.goto(current_url, wait_until="domcontentloaded")
        element = await page.main_frame.wait_for_selector("img")
        print("Loaded image: " + str(await element.get_attribute("src")))
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

## frame.wait_for_timeout(timeout)<a name="frame-wait-for-timeout">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> A timeout to wait for<a name="frame-wait-for-timeout-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-wait-for-timeout-return">#</a>

Waits for the given `timeout` in milliseconds.

Note that `frame.waitForTimeout()` should only be used for debugging. Tests using the timer in production are going to be flaky. Use signals such as network events, selectors becoming visible and others instead.

## frame.wait_for_url(url, **kwargs)<a name="frame-wait-for-url">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[URL](https://en.wikipedia.org/wiki/URL)]:[bool](https://docs.python.org/3/library/stdtypes.html)> A glob pattern, regex pattern or predicate receiving [URL](https://en.wikipedia.org/wiki/URL) to match while waiting for the navigation. Note that if the parameter is a string without wilcard characters, the method will wait for navigation to URL that is exactly equal to the string.<a name="frame-wait-for-url-option-url">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="frame-wait-for-url-option-timeout">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="frame-wait-for-url-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="frame-wait-for-url-return">#</a>

Waits for the frame to navigate to the given URL.

- Sync

```python
frame.click("a.delayed-navigation") # clicking the link will indirectly cause a navigation
frame.wait_for_url("**/target.html")
```

- Async

```python
await frame.click("a.delayed-navigation") # clicking the link will indirectly cause a navigation
await frame.wait_for_url("**/target.html")
```





# FrameLocator

FrameLocator represents a view to the `iframe` on the page. It captures the logic sufficient to retrieve the `iframe` and locate elements in that iframe. FrameLocator can be created with either [page.frame_locator(selector)](#page-frame-locator) or [locator.frame_locator(selector)](#locator-frame-locator) method.

- Sync

```python
locator = page.frame_locator("my-frame").locator("text=Submit")
locator.click()
```

- Async

```python
locator = page.frame_locator("#my-frame").locator("text=Submit")
await locator.click()
```

**Strictness**

Frame locators are strict. This means that all operations on frame locators will throw if more than one element matches given selector.

- Sync

```python
# Throws if there are several frames in DOM:
page.frame_locator('.result-frame').locator('button').click()

# Works because we explicitly tell locator to pick the first frame:
page.frame_locator('.result-frame').first.locator('button').click()
```

- Async

```python
# Throws if there are several frames in DOM:
await page.frame_locator('.result-frame').locator('button').click()

# Works because we explicitly tell locator to pick the first frame:
await page.frame_locator('.result-frame').first.locator('button').click()
```

**Converting Locator to FrameLocator**

If you have a [Locator](#locator) object pointing to an `iframe` it can be converted to [FrameLocator](#framelocator) using [`:scope`](https://developer.mozilla.org/en-US/docs/Web/CSS/:scope) CSS selector:

- Sync

```python
frameLocator = locator.frame_locator(":scope");
```

- Async

```python
frameLocator = locator.frame_locator(":scope");
```

- [frame_locator.first](#frame-locator-first)
- [frame_locator.frame_locator(selector)](#frame-locator-frame-locator)
- [frame_locator.last](#frame-locator-last)
- [frame_locator.locator(selector, **kwargs)](#frame-locator-locator)
- [frame_locator.nth(index)](#frame-locator-nth)

## frame_locator.first<a name="frame-locator-first">#</a>

- returns: \<[FrameLocator](#framelocator)><a name="frame-locator-first-return">#</a>

Returns locator to the first matching frame.

## frame_locator.frame_locator(selector)<a name="frame-locator-frame-locator">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to use when resolving DOM element. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-locator-frame-locator-option-selector">#</a>
- returns: \<[FrameLocator](#framelocator)><a name="frame-locator-frame-locator-return">#</a>

When working with iframes, you can create a frame locator that will enter the iframe and allow selecting elements in that iframe.

## frame_locator.last<a name="frame-locator-last">#</a>

- returns: \<[FrameLocator](#framelocator)><a name="frame-locator-last-return">#</a>

Returns locator to the last matching frame.

## frame_locator.locator(selector, **kwargs)<a name="frame-locator-locator">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to use when resolving DOM element. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="frame-locator-locator-option-selector">#</a>

- `has` \<[Locator](#locator)> Matches elements containing an element that matches an inner locator. Inner locator is queried against the outer one. For example, `article` that has `text=Playwright` matches `\<article>\<div>Playwright\</div>\</article>`.<a name="frame-locator-locator-option-has">#</a>

    Note that outer and inner locators must belong to the same frame. Inner locator must not contain [FrameLocator](#framelocator)s.

- `has_text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Matches elements containing specified text somewhere inside, possibly in a child or a descendant element. For example, `"Playwright"` matches `\<article>\<div>Playwright\</div>\</article>`.<a name="frame-locator-locator-option-has-text">#</a>

- returns: \<[Locator](#locator)><a name="frame-locator-locator-return">#</a>

The method finds an element matching the specified selector in the FrameLocator's subtree.

## frame_locator.nth(index)<a name="frame-locator-nth">#</a>

- `index` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="frame-locator-nth-option-index">#</a>
- returns: \<[FrameLocator](#framelocator)><a name="frame-locator-nth-return">#</a>

Returns locator to the n-th matching frame.





# JSHandle

JSHandle represents an in-page JavaScript object. JSHandles can be created with the [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle) method.

- Sync

```python
window_handle = page.evaluate_handle("window")
# ...
```

- Async

```python
window_handle = await page.evaluate_handle("window")
# ...
```

JSHandle prevents the referenced JavaScript object being garbage collected unless the handle is exposed with [js_handle.dispose()](#js-handle-dispose). JSHandles are auto-disposed when their origin frame gets navigated or the parent context gets destroyed.

JSHandle instances can be used as an argument in [page.eval_on_selector(selector, expression, **kwargs)](#page-eval-on-selector), [page.evaluate(expression, **kwargs)](#page-evaluate) and [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle) methods.

- [js_handle.as_element()](#js-handle-as-element)
- [js_handle.dispose()](#js-handle-dispose)
- [js_handle.evaluate(expression, **kwargs)](#js-handle-evaluate)
- [js_handle.evaluate_handle(expression, **kwargs)](#js-handle-evaluate-handle)
- [js_handle.get_properties()](#js-handle-get-properties)
- [js_handle.get_property(property_name)](#js-handle-get-property)
- [js_handle.json_value()](#js-handle-json-value)

## js_handle.as_element()<a name="js-handle-as-element">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[ElementHandle](#elementhandle)><a name="js-handle-as-element-return">#</a>

Returns either `null` or the object handle itself, if the object handle is an instance of [ElementHandle](#elementhandle).

## js_handle.dispose()<a name="js-handle-dispose">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="js-handle-dispose-return">#</a>

The `jsHandle.dispose` method stops referencing the element handle.

## js_handle.evaluate(expression, **kwargs)<a name="js-handle-evaluate">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="js-handle-evaluate-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="js-handle-evaluate-option-arg">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="js-handle-evaluate-return">#</a>

Returns the return value of `expression`.

This method passes this handle as the first argument to `expression`.

If `expression` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then `handle.evaluate` would wait for the promise to resolve and return its value.

Examples:

- Sync

```python
tweet_handle = page.query_selector(".tweet .retweets")
assert tweet_handle.evaluate("node => node.innerText") == "10 retweets"
```

- Async

```python
tweet_handle = await page.query_selector(".tweet .retweets")
assert await tweet_handle.evaluate("node => node.innerText") == "10 retweets"
```

## js_handle.evaluate_handle(expression, **kwargs)<a name="js-handle-evaluate-handle">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="js-handle-evaluate-handle-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="js-handle-evaluate-handle-option-arg">#</a>
- returns: \<[JSHandle](#jshandle)><a name="js-handle-evaluate-handle-return">#</a>

Returns the return value of `expression` as a [JSHandle](#jshandle).

This method passes this handle as the first argument to `expression`.

The only difference between `jsHandle.evaluate` and `jsHandle.evaluateHandle` is that `jsHandle.evaluateHandle` returns [JSHandle](#jshandle).

If the function passed to the `jsHandle.evaluateHandle` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then `jsHandle.evaluateHandle` would wait for the promise to resolve and return its value.

See [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle) for more details.

## js_handle.get_properties()<a name="js-handle-get-properties">#</a>

- returns: \<[Map][[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [JSHandle](#jshandle)]><a name="js-handle-get-properties-return">#</a>

The method returns a map with **own property names** as keys and JSHandle instances for the property values.

- Sync

```python
handle = page.evaluate_handle("{window, document}")
properties = handle.get_properties()
window_handle = properties.get("window")
document_handle = properties.get("document")
handle.dispose()
```

- Async

```python
handle = await page.evaluate_handle("{window, document}")
properties = await handle.get_properties()
window_handle = properties.get("window")
document_handle = properties.get("document")
await handle.dispose()
```

## js_handle.get_property(property_name)<a name="js-handle-get-property">#</a>

- `property_name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> property to get<a name="js-handle-get-property-option-property-name">#</a>
- returns: \<[JSHandle](#jshandle)><a name="js-handle-get-property-return">#</a>

Fetches a single property from the referenced object.

## js_handle.json_value()<a name="js-handle-json-value">#</a>

- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="js-handle-json-value-return">#</a>

Returns a JSON representation of the object. If the object has a `toJSON` function, it **will not be called**.

> NOTE
>
> The method will return an empty JSON object if the referenced object is not stringifiable. It will throw an error if the object has circular references.





# Keyboard

Keyboard provides an api for managing a virtual keyboard. The high level api is [keyboard.type(text, **kwargs)](#keyboard-type), which takes raw characters and generates proper keydown, keypress/input, and keyup events on your page.

For finer control, you can use [keyboard.down(key)](#keyboard-down), [keyboard.up(key)](#keyboard-up), and [keyboard.insert_text(text)](#keyboard-insert-text) to manually fire events as if they were generated from a real keyboard.

An example of holding down `Shift` in order to select and delete some text:

- Sync

```python
page.keyboard.type("Hello World!")
page.keyboard.press("ArrowLeft")
page.keyboard.down("Shift")
for i in range(6):
    page.keyboard.press("ArrowLeft")
page.keyboard.up("Shift")
page.keyboard.press("Backspace")
# result text will end up saying "Hello!"
```

- Async

```python
await page.keyboard.type("Hello World!")
await page.keyboard.press("ArrowLeft")
await page.keyboard.down("Shift")
for i in range(6):
    await page.keyboard.press("ArrowLeft")
await page.keyboard.up("Shift")
await page.keyboard.press("Backspace")
# result text will end up saying "Hello!"
```

An example of pressing uppercase `A`

- Sync

```python
page.keyboard.press("Shift+KeyA")
# or
page.keyboard.press("Shift+A")
```

- Async

```python
await page.keyboard.press("Shift+KeyA")
# or
await page.keyboard.press("Shift+A")
```

An example to trigger select-all with the keyboard

- Sync

```python
# on windows and linux
page.keyboard.press("Control+A")
# on mac_os
page.keyboard.press("Meta+A")
```

- Async

```python
# on windows and linux
await page.keyboard.press("Control+A")
# on mac_os
await page.keyboard.press("Meta+A")
```

- [keyboard.down(key)](#keyboard-down)
- [keyboard.insert_text(text)](#keyboard-insert-text)
- [keyboard.press(key, **kwargs)](#keyboard-press)
- [keyboard.type(text, **kwargs)](#keyboard-type)
- [keyboard.up(key)](#keyboard-up)

## keyboard.down(key)<a name="keyboard-down">#</a>

- `key` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the key to press or a character to generate, such as `ArrowLeft` or `a`.<a name="keyboard-down-option-key">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="keyboard-down-return">#</a>

Dispatches a `keydown` event.

`key` can specify the intended [keyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key) value or a single character to generate the text for. A superset of the `key` values can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values). Examples of the keys are:

`F1` - `F12`, `Digit0`- `Digit9`, `KeyA`- `KeyZ`, `Backquote`, `Minus`, `Equal`, `Backslash`, `Backspace`, `Tab`, `Delete`, `Escape`, `ArrowDown`, `End`, `Enter`, `Home`, `Insert`, `PageDown`, `PageUp`, `ArrowRight`, `ArrowUp`, etc.

Following modification shortcuts are also supported: `Shift`, `Control`, `Alt`, `Meta`, `ShiftLeft`.

Holding down `Shift` will type the text that corresponds to the `key` in the upper case.

If `key` is a single character, it is case-sensitive, so the values `a` and `A` will generate different respective texts.

If `key` is a modifier key, `Shift`, `Meta`, `Control`, or `Alt`, subsequent key presses will be sent with that modifier active. To release the modifier key, use [keyboard.up(key)](#keyboard-up).

After the key is pressed once, subsequent calls to [keyboard.down(key)](#keyboard-down) will have [repeat](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/repeat) set to true. To release the key, use [keyboard.up(key)](#keyboard-up).

> NOTE
>
> Modifier keys DO influence `keyboard.down`. Holding down `Shift` will type the text in upper case.

## keyboard.insert_text(text)<a name="keyboard-insert-text">#</a>

- `text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Sets input to the specified text value.<a name="keyboard-insert-text-option-text">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="keyboard-insert-text-return">#</a>

Dispatches only `input` event, does not emit the `keydown`, `keyup` or `keypress` events.

- Sync

```python
page.keyboard.insert_text("嗨")
```

- Async

```python
await page.keyboard.insert_text("嗨")
```

> NOTE
>
> Modifier keys DO NOT effect `keyboard.insertText`. Holding down `Shift` will not type the text in upper case.

## keyboard.press(key, **kwargs)<a name="keyboard-press">#</a>

- `key` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the key to press or a character to generate, such as `ArrowLeft` or `a`.<a name="keyboard-press-option-key">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `keydown` and `keyup` in milliseconds. Defaults to 0.<a name="keyboard-press-option-delay">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="keyboard-press-return">#</a>

`key` can specify the intended [keyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key) value or a single character to generate the text for. A superset of the `key` values can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values). Examples of the keys are:

`F1` - `F12`, `Digit0`- `Digit9`, `KeyA`- `KeyZ`, `Backquote`, `Minus`, `Equal`, `Backslash`, `Backspace`, `Tab`, `Delete`, `Escape`, `ArrowDown`, `End`, `Enter`, `Home`, `Insert`, `PageDown`, `PageUp`, `ArrowRight`, `ArrowUp`, etc.

Following modification shortcuts are also supported: `Shift`, `Control`, `Alt`, `Meta`, `ShiftLeft`.

Holding down `Shift` will type the text that corresponds to the `key` in the upper case.

If `key` is a single character, it is case-sensitive, so the values `a` and `A` will generate different respective texts.

Shortcuts such as `key: "Control+o"` or `key: "Control+Shift+T"` are supported as well. When specified with the modifier, modifier is pressed and being held while the subsequent key is being pressed.

- Sync

```python
page = browser.new_page()
page.goto("https://keycode.info")
page.keyboard.press("a")
page.screenshot(path="a.png")
page.keyboard.press("ArrowLeft")
page.screenshot(path="arrow_left.png")
page.keyboard.press("Shift+O")
page.screenshot(path="o.png")
browser.close()
```

- Async

```python
page = await browser.new_page()
await page.goto("https://keycode.info")
await page.keyboard.press("a")
await page.screenshot(path="a.png")
await page.keyboard.press("ArrowLeft")
await page.screenshot(path="arrow_left.png")
await page.keyboard.press("Shift+O")
await page.screenshot(path="o.png")
await browser.close()
```

Shortcut for [keyboard.down(key)](#keyboard-down) and [keyboard.up(key)](#keyboard-up).

## keyboard.type(text, **kwargs)<a name="keyboard-type">#</a>

- `text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A text to type into a focused element.<a name="keyboard-type-option-text">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between key presses in milliseconds. Defaults to 0.<a name="keyboard-type-option-delay">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="keyboard-type-return">#</a>

Sends a `keydown`, `keypress`/`input`, and `keyup` event for each character in the text.

To press a special key, like `Control` or `ArrowDown`, use [keyboard.press(key, **kwargs)](#keyboard-press).

- Sync

```python
page.keyboard.type("Hello") # types instantly
page.keyboard.type("World", delay=100) # types slower, like a user
```

- Async

```python
await page.keyboard.type("Hello") # types instantly
await page.keyboard.type("World", delay=100) # types slower, like a user
```

> NOTE
>
> Modifier keys DO NOT effect `keyboard.type`. Holding down `Shift` will not type the text in upper case.

> NOTE
>
> For characters that are not on a US keyboard, only an `input` event will be sent.

## keyboard.up(key)<a name="keyboard-up">#</a>

- `key` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the key to press or a character to generate, such as `ArrowLeft` or `a`.<a name="keyboard-up-option-key">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="keyboard-up-return">#</a>

Dispatches a `keyup` event.



# Locator

Locators are the central piece of Playwright's auto-waiting and retry-ability. In a nutshell, locators represent a way to find element(s) on the page at any moment. Locator can be created with the [page.locator(selector, **kwargs)](#page-locator) method.

[Learn more about locators](https://playwright.dev/python/docs/locators).

- [locator.all_inner_texts()](#locator-all-inner-texts)
- [locator.all_text_contents()](#locator-all-text-contents)
- [locator.bounding_box(**kwargs)](#locator-bounding-box)
- [locator.check(**kwargs)](#locator-check)
- [locator.click(**kwargs)](#locator-click)
- [locator.count()](#locator-count)
- [locator.dblclick(**kwargs)](#locator-dblclick)
- [locator.dispatch_event(type, **kwargs)](#locator-dispatch-event)
- [locator.drag_to(target, **kwargs)](#locator-drag-to)
- [locator.element_handle(**kwargs)](#locator-element-handle)
- [locator.element_handles()](#locator-element-handles)
- [locator.evaluate(expression, **kwargs)](#locator-evaluate)
- [locator.evaluate_all(expression, **kwargs)](#locator-evaluate-all)
- [locator.evaluate_handle(expression, **kwargs)](#locator-evaluate-handle)
- [locator.fill(value, **kwargs)](#locator-fill)
- [locator.first](#locator-first)
- [locator.focus(**kwargs)](#locator-focus)
- [locator.frame_locator(selector)](#locator-frame-locator)
- [locator.get_attribute(name, **kwargs)](#locator-get-attribute)
- [locator.highlight()](#locator-highlight)
- [locator.hover(**kwargs)](#locator-hover)
- [locator.inner_html(**kwargs)](#locator-inner-html)
- [locator.inner_text(**kwargs)](#locator-inner-text)
- [locator.input_value(**kwargs)](#locator-input-value)
- [locator.is_checked(**kwargs)](#locator-is-checked)
- [locator.is_disabled(**kwargs)](#locator-is-disabled)
- [locator.is_editable(**kwargs)](#locator-is-editable)
- [locator.is_enabled(**kwargs)](#locator-is-enabled)
- [locator.is_hidden(**kwargs)](#locator-is-hidden)
- [locator.is_visible(**kwargs)](#locator-is-visible)
- [locator.last](#locator-last)
- [locator.locator(selector, **kwargs)](#locator-locator)
- [locator.nth(index)](#locator-nth)
- [locator.page](#locator-page)
- [locator.press(key, **kwargs)](#locator-press)
- [locator.screenshot(**kwargs)](#locator-screenshot)
- [locator.scroll_into_view_if_needed(**kwargs)](#locator-scroll-into-view-if-needed)
- [locator.select_option(**kwargs)](#locator-select-option)
- [locator.select_text(**kwargs)](#locator-select-text)
- [locator.set_checked(checked, **kwargs)](#locator-set-checked)
- [locator.set_input_files(files, **kwargs)](#locator-set-input-files)
- [locator.tap(**kwargs)](#locator-tap)
- [locator.text_content(**kwargs)](#locator-text-content)
- [locator.type(text, **kwargs)](#locator-type)
- [locator.uncheck(**kwargs)](#locator-uncheck)
- [locator.wait_for(**kwargs)](#locator-wait-for)

## locator.all_inner_texts()<a name="locator-all-inner-texts">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="locator-all-inner-texts-return">#</a>

Returns an array of `node.innerText` values for all matching nodes.

## locator.all_text_contents()<a name="locator-all-text-contents">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="locator-all-text-contents-return">#</a>

Returns an array of `node.textContent` values for all matching nodes.

## locator.bounding_box(**kwargs)<a name="locator-bounding-box">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-bounding-box-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="locator-bounding-box-return">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> the x coordinate of the element in pixels.
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> the y coordinate of the element in pixels.
    - `width` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> the width of the element in pixels.
    - `height` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> the height of the element in pixels.

This method returns the bounding box of the element, or `null` if the element is not visible. The bounding box is calculated relative to the main frame viewport - which is usually the same as the browser window.

Scrolling affects the returned bonding box, similarly to [Element.getBoundingClientRect](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect). That means `x` and/or `y` may be negative.

Elements from child frames return the bounding box relative to the main frame, unlike the [Element.getBoundingClientRect](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect).

Assuming the page is static, it is safe to use bounding box coordinates to perform input. For example, the following snippet should click the center of the element.

- Sync

```python
box = element.bounding_box()
page.mouse.click(box["x"] + box["width"] / 2, box["y"] + box["height"] / 2)
```

- Async

```python
box = await element.bounding_box()
await page.mouse.click(box["x"] + box["width"] / 2, box["y"] + box["height"] / 2)
```

## locator.check(**kwargs)<a name="locator-check">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-check-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-check-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="locator-check-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-check-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="locator-check-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-check-return">#</a>

This method checks the element by performing the following steps:

1. Ensure that element is a checkbox or a radio input. If not, this method throws. If the element is already checked, this method returns immediately.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to click in the center of the element.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
6. Ensure that the element is now checked. If not, this method throws.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## locator.click(**kwargs)<a name="locator-click">#</a>

- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="locator-click-option-button">#</a>
- `click_count` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> defaults to 1. See [UIEvent.detail](https://developer.mozilla.org/en-US/docs/Web/API/UIEvent/detail).<a name="locator-click-option-click-count">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="locator-click-option-delay">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-click-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="locator-click-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-click-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="locator-click-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-click-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="locator-click-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-click-return">#</a>

This method clicks the element by performing the following steps:

1. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
2. Scroll the element into view if needed.
3. Use [page.mouse](#page-mouse) to click in the center of the element, or the specified `position`.
4. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## locator.count()<a name="locator-count">#</a>

- returns: \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="locator-count-return">#</a>

Returns the number of elements matching given selector.

## locator.dblclick(**kwargs)<a name="locator-dblclick">#</a>

- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="locator-dblclick-option-button">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="locator-dblclick-option-delay">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-dblclick-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="locator-dblclick-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-dblclick-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="locator-dblclick-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-dblclick-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="locator-dblclick-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-dblclick-return">#</a>

This method double clicks the element by performing the following steps:

1. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
2. Scroll the element into view if needed.
3. Use [page.mouse](#page-mouse) to double click in the center of the element, or the specified `position`.
4. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set. Note that if the first click of the `dblclick()` triggers a navigation event, this method will throw.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

> NOTE
>
> `element.dblclick()` dispatches two `click` events and a single `dblclick` event.

## locator.dispatch_event(type, **kwargs)<a name="locator-dispatch-event">#</a>

- `type` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> DOM event type: `"click"`, `"dragstart"`, etc.<a name="locator-dispatch-event-option-type">#</a>
- `event_init` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional event-specific initialization properties.<a name="locator-dispatch-event-option-event-init">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-dispatch-event-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-dispatch-event-return">#</a>

The snippet below dispatches the `click` event on the element. Regardless of the visibility state of the element, `click` is dispatched. This is equivalent to calling [element.click()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/click).

- Sync

```python
element.dispatch_event("click")
```

- Async

```python
await element.dispatch_event("click")
```

Under the hood, it creates an instance of an event based on the given `type`, initializes it with `event_init` properties and dispatches it on the element. Events are `composed`, `cancelable` and bubble by default.

Since `event_init` is event-specific, please refer to the events documentation for the lists of initial properties:

- [DragEvent](https://developer.mozilla.org/en-US/docs/Web/API/DragEvent/DragEvent)
- [FocusEvent](https://developer.mozilla.org/en-US/docs/Web/API/FocusEvent/FocusEvent)
- [KeyboardEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/KeyboardEvent)
- [MouseEvent](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/MouseEvent)
- [PointerEvent](https://developer.mozilla.org/en-US/docs/Web/API/PointerEvent/PointerEvent)
- [TouchEvent](https://developer.mozilla.org/en-US/docs/Web/API/TouchEvent/TouchEvent)
- [Event](https://developer.mozilla.org/en-US/docs/Web/API/Event/Event)

You can also specify `JSHandle` as the property value if you want live objects to be passed into the event:

- Sync

```python
# note you can only create data_transfer in chromium and firefox
data_transfer = page.evaluate_handle("new DataTransfer()")
element.dispatch_event("#source", "dragstart", {"dataTransfer": data_transfer})
```

- Async

```python
# note you can only create data_transfer in chromium and firefox
data_transfer = await page.evaluate_handle("new DataTransfer()")
await element.dispatch_event("#source", "dragstart", {"dataTransfer": data_transfer})
```

## locator.drag_to(target, **kwargs)<a name="locator-drag-to">#</a>

- `target` \<[Locator](#locator)> Locator of the element to drag to.<a name="locator-drag-to-option-target">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-drag-to-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-drag-to-option-no-wait-after">#</a>
- `source_position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Clicks on the source element at this point relative to the top-left corner of the element's padding box. If not specified, some visible point of the element is used.<a name="locator-drag-to-option-source-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `target_position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Drops on the target element at this point relative to the top-left corner of the element's padding box. If not specified, some visible point of the element is used.<a name="locator-drag-to-option-target-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-drag-to-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="locator-drag-to-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-drag-to-return">#</a>

## locator.element_handle(**kwargs)<a name="locator-element-handle">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-element-handle-option-timeout">#</a>
- returns: \<[ElementHandle](#elementhandle)><a name="locator-element-handle-return">#</a>

Resolves given locator to the first matching DOM element. If no elements matching the query are visible, waits for them up to a given timeout. If multiple elements match the selector, throws.

## locator.element_handles()<a name="locator-element-handles">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[ElementHandle](#elementhandle)]><a name="locator-element-handles-return">#</a>

Resolves given locator to all matching DOM elements.

## locator.evaluate(expression, **kwargs)<a name="locator-evaluate">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="locator-evaluate-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="locator-evaluate-option-arg">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-evaluate-option-timeout">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="locator-evaluate-return">#</a>

Returns the return value of `expression`.

This method passes this handle as the first argument to `expression`.

If `expression` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then `handle.evaluate` would wait for the promise to resolve and return its value.

Examples:

- Sync

```python
tweets = page.locator(".tweet .retweets")
assert tweets.evaluate("node => node.innerText") == "10 retweets"
```

- Async

```python
tweets = page.locator(".tweet .retweets")
assert await tweets.evaluate("node => node.innerText") == "10 retweets"
```

## locator.evaluate_all(expression, **kwargs)<a name="locator-evaluate-all">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="locator-evaluate-all-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="locator-evaluate-all-option-arg">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="locator-evaluate-all-return">#</a>

The method finds all elements matching the specified locator and passes an array of matched elements as a first argument to `expression`. Returns the result of `expression` invocation.

If `expression` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [locator.evaluate_all(expression, **kwargs)](#locator-evaluate-all) would wait for the promise to resolve and return its value.

Examples:

- Sync

```python
elements = page.locator("div")
div_counts = elements("(divs, min) => divs.length >= min", 10)
```

- Async

```python
elements = page.locator("div")
div_counts = await elements("(divs, min) => divs.length >= min", 10)
```

## locator.evaluate_handle(expression, **kwargs)<a name="locator-evaluate-handle">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="locator-evaluate-handle-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="locator-evaluate-handle-option-arg">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-evaluate-handle-option-timeout">#</a>
- returns: \<[JSHandle](#jshandle)><a name="locator-evaluate-handle-return">#</a>

Returns the return value of `expression` as a [JSHandle](#jshandle).

This method passes this handle as the first argument to `expression`.

The only difference between [locator.evaluate(expression, **kwargs)](#locator-evaluate) and [locator.evaluate_handle(expression, **kwargs)](#locator-evaluate-handle) is that [locator.evaluate_handle(expression, **kwargs)](#locator-evaluate-handle) returns [JSHandle](#jshandle).

If the function passed to the [locator.evaluate_handle(expression, **kwargs)](#locator-evaluate-handle) returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [locator.evaluate_handle(expression, **kwargs)](#locator-evaluate-handle) would wait for the promise to resolve and return its value.

See [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle) for more details.

## locator.fill(value, **kwargs)<a name="locator-fill">#</a>

- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Value to set for the `\<input>`, `\<textarea>` or `[contenteditable]` element.<a name="locator-fill-option-value">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-fill-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-fill-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-fill-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-fill-return">#</a>

This method waits for [actionability](https://playwright.dev/python/docs/actionability) checks, focuses the element, fills it and triggers an `input` event after filling. Note that you can pass an empty string to clear the input field.

If the target element is not an `\<input>`, `\<textarea>` or `[contenteditable]` element, this method throws an error. However, if the element is inside the `\<label>` element that has an associated [control](https://developer.mozilla.org/en-US/docs/Web/API/HTMLLabelElement/control), the control will be filled instead.

To send fine-grained keyboard events, use [locator.type(text, **kwargs)](#locator-type).

## locator.first<a name="locator-first">#</a>

- returns: \<[Locator](#locator)><a name="locator-first-return">#</a>

Returns locator to the first matching element.

## locator.focus(**kwargs)<a name="locator-focus">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-focus-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-focus-return">#</a>

Calls [focus](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus) on the element.

## locator.frame_locator(selector)<a name="locator-frame-locator">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to use when resolving DOM element. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="locator-frame-locator-option-selector">#</a>
- returns: \<[FrameLocator](#framelocator)><a name="locator-frame-locator-return">#</a>

When working with iframes, you can create a frame locator that will enter the iframe and allow selecting elements in that iframe:

- Sync

```python
locator = page.frame_locator("iframe").locator("text=Submit")
locator.click()
```

- Async

```python
locator = page.frame_locator("iframe").locator("text=Submit")
await locator.click()
```

## locator.get_attribute(name, **kwargs)<a name="locator-get-attribute">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Attribute name to get the value for.<a name="locator-get-attribute-option-name">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-get-attribute-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="locator-get-attribute-return">#</a>

Returns element attribute value.

## locator.highlight()<a name="locator-highlight">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-highlight-return">#</a>

Highlight the corresponding element(s) on the screen. Useful for debugging, don't commit the code that uses [locator.highlight()](#locator-highlight).

## locator.hover(**kwargs)<a name="locator-hover">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-hover-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="locator-hover-option-modifiers">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="locator-hover-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-hover-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="locator-hover-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-hover-return">#</a>

This method hovers over the element by performing the following steps:

1. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
2. Scroll the element into view if needed.
3. Use [page.mouse](#page-mouse) to hover over the center of the element, or the specified `position`.
4. Wait for initiated navigations to either succeed or fail, unless `noWaitAfter` option is set.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## locator.inner_html(**kwargs)<a name="locator-inner-html">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-inner-html-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="locator-inner-html-return">#</a>

Returns the `element.innerHTML`.

## locator.inner_text(**kwargs)<a name="locator-inner-text">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-inner-text-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="locator-inner-text-return">#</a>

Returns the `element.innerText`.

## locator.input_value(**kwargs)<a name="locator-input-value">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-input-value-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="locator-input-value-return">#</a>

Returns `input.value` for `\<input>` or `\<textarea>` or `\<select>` element. Throws for non-input elements.

## locator.is_checked(**kwargs)<a name="locator-is-checked">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-is-checked-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="locator-is-checked-return">#</a>

Returns whether the element is checked. Throws if the element is not a checkbox or radio input.

## locator.is_disabled(**kwargs)<a name="locator-is-disabled">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-is-disabled-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="locator-is-disabled-return">#</a>

Returns whether the element is disabled, the opposite of [enabled](https://playwright.dev/python/docs/actionability#enabled).

## locator.is_editable(**kwargs)<a name="locator-is-editable">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-is-editable-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="locator-is-editable-return">#</a>

Returns whether the element is [editable](https://playwright.dev/python/docs/actionability#editable).

## locator.is_enabled(**kwargs)<a name="locator-is-enabled">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-is-enabled-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="locator-is-enabled-return">#</a>

Returns whether the element is [enabled](https://playwright.dev/python/docs/actionability#enabled).

## locator.is_hidden(**kwargs)<a name="locator-is-hidden">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> **DEPRECATED** This option is ignored. [locator.is_hidden(**kwargs)](#locator-is-hidden) does not wait for the element to become hidden and returns immediately.<a name="locator-is-hidden-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="locator-is-hidden-return">#</a>

Returns whether the element is hidden, the opposite of [visible](https://playwright.dev/python/docs/actionability#visible).

## locator.is_visible(**kwargs)<a name="locator-is-visible">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> **DEPRECATED** This option is ignored. [locator.is_visible(**kwargs)](#locator-is-visible) does not wait for the element to become visible and returns immediately.<a name="locator-is-visible-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="locator-is-visible-return">#</a>

Returns whether the element is [visible](https://playwright.dev/python/docs/actionability#visible).

## locator.last<a name="locator-last">#</a>

- returns: \<[Locator](#locator)><a name="locator-last-return">#</a>

Returns locator to the last matching element.

## locator.locator(selector, **kwargs)<a name="locator-locator">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to use when resolving DOM element. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="locator-locator-option-selector">#</a>

- `has` \<[Locator](#locator)> Matches elements containing an element that matches an inner locator. Inner locator is queried against the outer one. For example, `article` that has `text=Playwright` matches `\<article>\<div>Playwright\</div>\</article>`.<a name="locator-locator-option-has">#</a>

    Note that outer and inner locators must belong to the same frame. Inner locator must not contain [FrameLocator](#framelocator)s.

- `has_text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Matches elements containing specified text somewhere inside, possibly in a child or a descendant element. For example, `"Playwright"` matches `\<article>\<div>Playwright\</div>\</article>`.<a name="locator-locator-option-has-text">#</a>

- returns: \<[Locator](#locator)><a name="locator-locator-return">#</a>

The method finds an element matching the specified selector in the `Locator`'s subtree.

## locator.nth(index)<a name="locator-nth">#</a>

- `index` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="locator-nth-option-index">#</a>
- returns: \<[Locator](#locator)><a name="locator-nth-return">#</a>

Returns locator to the n-th matching element.

## locator.page<a name="locator-page">#</a>

- returns: \<[Page](#page)><a name="locator-page-return">#</a>

A page this locator belongs to.

## locator.press(key, **kwargs)<a name="locator-press">#</a>

- `key` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the key to press or a character to generate, such as `ArrowLeft` or `a`.<a name="locator-press-option-key">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `keydown` and `keyup` in milliseconds. Defaults to 0.<a name="locator-press-option-delay">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-press-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-press-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-press-return">#</a>

Focuses the element, and then uses [keyboard.down(key)](#keyboard-down) and [keyboard.up(key)](#keyboard-up).

`key` can specify the intended [keyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key) value or a single character to generate the text for. A superset of the `key` values can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values). Examples of the keys are:

`F1` - `F12`, `Digit0`- `Digit9`, `KeyA`- `KeyZ`, `Backquote`, `Minus`, `Equal`, `Backslash`, `Backspace`, `Tab`, `Delete`, `Escape`, `ArrowDown`, `End`, `Enter`, `Home`, `Insert`, `PageDown`, `PageUp`, `ArrowRight`, `ArrowUp`, etc.

Following modification shortcuts are also supported: `Shift`, `Control`, `Alt`, `Meta`, `ShiftLeft`.

Holding down `Shift` will type the text that corresponds to the `key` in the upper case.

If `key` is a single character, it is case-sensitive, so the values `a` and `A` will generate different respective texts.

Shortcuts such as `key: "Control+o"` or `key: "Control+Shift+T"` are supported as well. When specified with the modifier, modifier is pressed and being held while the subsequent key is being pressed.

## locator.screenshot(**kwargs)<a name="locator-screenshot">#</a>

- `animations` \<"disabled"> When set to `"disabled"`, stops CSS animations, CSS transitions and Web Animations. Animations get different treatment depending on their duration:<a name="locator-screenshot-option-animations">#</a>
    - finite animations are fast-forwarded to completion, so they'll fire `transitionend` event.
    - infinite animations are canceled to initial state, and then played over after the screenshot.
- `mask` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Locator](#locator)]> Specify locators that should be masked when the screenshot is taken. Masked elements will be overlayed with a pink box `#FF00FF` that completely covers its bounding box.<a name="locator-screenshot-option-mask">#</a>
- `omit_background` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Hides default white background and allows capturing screenshots with transparency. Not applicable to `jpeg` images. Defaults to `false`.<a name="locator-screenshot-option-omit-background">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> The file path to save the image to. The screenshot type will be inferred from file extension. If `path` is a relative path, then it is resolved relative to the current working directory. If no path is provided, the image won't be saved to the disk.<a name="locator-screenshot-option-path">#</a>
- `quality` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> The quality of the image, between 0-100. Not applicable to `png` images.<a name="locator-screenshot-option-quality">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-screenshot-option-timeout">#</a>
- `type` \<"png"|"jpeg"> Specify screenshot type, defaults to `png`.<a name="locator-screenshot-option-type">#</a>
- returns: \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)><a name="locator-screenshot-return">#</a>

Returns the buffer with the captured screenshot.

This method waits for the [actionability](https://playwright.dev/python/docs/actionability) checks, then scrolls element into view before taking a screenshot. If the element is detached from DOM, the method throws an error.

## locator.scroll_into_view_if_needed(**kwargs)<a name="locator-scroll-into-view-if-needed">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-scroll-into-view-if-needed-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-scroll-into-view-if-needed-return">#</a>

This method waits for [actionability](https://playwright.dev/python/docs/actionability) checks, then tries to scroll element into view, unless it is completely visible as defined by [IntersectionObserver](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)'s `ratio`.

## locator.select_option(**kwargs)<a name="locator-select-option">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-select-option-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-select-option-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-select-option-option-timeout">#</a>
- `element` \<[ElementHandle](#elementhandle)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[ElementHandle](#elementhandle)]> Option elements to select. Optional.<a name="locator-select-option-option-element">#</a>
- `index` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)]> Options to select by index. Optional.<a name="locator-select-option-option-index">#</a>
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Options to select by value. If the `\<select>` has the `multiple` attribute, all given options are selected, otherwise only the first option matching one of the passed options is selected. Optional.<a name="locator-select-option-option-value">#</a>
- `label` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Options to select by label. If the `\<select>` has the `multiple` attribute, all given options are selected, otherwise only the first option matching one of the passed options is selected. Optional.<a name="locator-select-option-option-label">#</a>
- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="locator-select-option-return">#</a>

This method waits for [actionability](https://playwright.dev/python/docs/actionability) checks, waits until all specified options are present in the `\<select>` element and selects these options.

If the target element is not a `\<select>` element, this method throws an error. However, if the element is inside the `\<label>` element that has an associated [control](https://developer.mozilla.org/en-US/docs/Web/API/HTMLLabelElement/control), the control will be used instead.

Returns the array of option values that have been successfully selected.

Triggers a `change` and `input` event once all the provided options have been selected.

- Sync

```python
# single selection matching the value
element.select_option("blue")
# single selection matching both the label
element.select_option(label="blue")
# multiple selection
element.select_option(value=["red", "green", "blue"])
```

- Async

```python
# single selection matching the value
await element.select_option("blue")
# single selection matching the label
await element.select_option(label="blue")
# multiple selection
await element.select_option(value=["red", "green", "blue"])
```

```python
# sync

# single selection matching the value
element.select_option("blue")
# single selection matching both the value and the label
element.select_option(label="blue")
# multiple selection
element.select_option("red", "green", "blue")
# multiple selection for blue, red and second option
element.select_option(value="blue", { index: 2 }, "red")
```

## locator.select_text(**kwargs)<a name="locator-select-text">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-select-text-option-force">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-select-text-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-select-text-return">#</a>

This method waits for [actionability](https://playwright.dev/python/docs/actionability) checks, then focuses the element and selects all its text content.

## locator.set_checked(checked, **kwargs)<a name="locator-set-checked">#</a>

- `checked` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to check or uncheck the checkbox.<a name="locator-set-checked-option-checked">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-set-checked-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-set-checked-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="locator-set-checked-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-set-checked-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="locator-set-checked-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-set-checked-return">#</a>

This method checks or unchecks an element by performing the following steps:

1. Ensure that matched element is a checkbox or a radio input. If not, this method throws.
2. If the element already has the right checked state, this method returns immediately.
3. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
4. Scroll the element into view if needed.
5. Use [page.mouse](#page-mouse) to click in the center of the element.
6. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
7. Ensure that the element is now checked or unchecked. If not, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## locator.set_input_files(files, **kwargs)<a name="locator-set-input-files">#</a>

- `files` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="locator-set-input-files-option-files">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-set-input-files-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-set-input-files-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-set-input-files-return">#</a>

This method expects `element` to point to an [input element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input).

Sets the value of the file input to these file paths or files. If some of the `filePaths` are relative paths, then they are resolved relative to the the current working directory. For empty array, clears the selected files.

## locator.tap(**kwargs)<a name="locator-tap">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-tap-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="locator-tap-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-tap-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="locator-tap-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-tap-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="locator-tap-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-tap-return">#</a>

This method taps the element by performing the following steps:

1. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
2. Scroll the element into view if needed.
3. Use [page.touchscreen](#page-touchscreen) to tap the center of the element, or the specified `position`.
4. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

> NOTE
>
> `element.tap()` requires that the `hasTouch` option of the browser context be set to true.

## locator.text_content(**kwargs)<a name="locator-text-content">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-text-content-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="locator-text-content-return">#</a>

Returns the `node.textContent`.

## locator.type(text, **kwargs)<a name="locator-type">#</a>

- `text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A text to type into a focused element.<a name="locator-type-option-text">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between key presses in milliseconds. Defaults to 0.<a name="locator-type-option-delay">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-type-option-no-wait-after">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-type-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-type-return">#</a>

Focuses the element, and then sends a `keydown`, `keypress`/`input`, and `keyup` event for each character in the text.

To press a special key, like `Control` or `ArrowDown`, use [locator.press(key, **kwargs)](#locator-press).

- Sync

```python
element.type("hello") # types instantly
element.type("world", delay=100) # types slower, like a user
```

- Async

```python
await element.type("hello") # types instantly
await element.type("world", delay=100) # types slower, like a user
```

An example of typing into a text field and then submitting the form:

- Sync

```python
element = page.locator("input")
element.type("some text")
element.press("Enter")
```

- Async

```python
element = page.locator("input")
await element.type("some text")
await element.press("Enter")
```

## locator.uncheck(**kwargs)<a name="locator-uncheck">#</a>

- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="locator-uncheck-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="locator-uncheck-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="locator-uncheck-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-uncheck-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="locator-uncheck-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-uncheck-return">#</a>

This method checks the element by performing the following steps:

1. Ensure that element is a checkbox or a radio input. If not, this method throws. If the element is already unchecked, this method returns immediately.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the element, unless `force` option is set.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to click in the center of the element.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
6. Ensure that the element is now unchecked. If not, this method throws.

If the element is detached from the DOM at any moment during the action, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

## locator.wait_for(**kwargs)<a name="locator-wait-for">#</a>

- `state` \<"attached"|"detached"|"visible"|"hidden"> Defaults to `'visible'`. Can be either:<a name="locator-wait-for-option-state">#</a>
    - `'attached'` - wait for element to be present in DOM.
    - `'detached'` - wait for element to not be present in DOM.
    - `'visible'` - wait for element to have non-empty bounding box and no `visibility:hidden`. Note that element without any content or with `display:none` has an empty bounding box and is not considered visible.
    - `'hidden'` - wait for element to be either detached from DOM, or have an empty bounding box or `visibility:hidden`. This is opposite to the `'visible'` option.
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="locator-wait-for-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="locator-wait-for-return">#</a>

Returns when element specified by locator satisfies the `state` option.

If target element already satisfies the condition, the method returns immediately. Otherwise, waits for up to `timeout` milliseconds until the condition is met.

- Sync

```python
order_sent = page.locator("#order-sent")
order_sent.wait_for()
```

- Async

```python
order_sent = page.locator("#order-sent")
await order_sent.wait_for()
```





# Mouse

The Mouse class operates in main-frame CSS pixels relative to the top-left corner of the viewport.

Every `page` object has its own Mouse, accessible with [page.mouse](#page-mouse).

- Sync

```python
# using ‘page.mouse’ to trace a 100x100 square.
page.mouse.move(0, 0)
page.mouse.down()
page.mouse.move(0, 100)
page.mouse.move(100, 100)
page.mouse.move(100, 0)
page.mouse.move(0, 0)
page.mouse.up()
```

- Async

```python
# using ‘page.mouse’ to trace a 100x100 square.
await page.mouse.move(0, 0)
await page.mouse.down()
await page.mouse.move(0, 100)
await page.mouse.move(100, 100)
await page.mouse.move(100, 0)
await page.mouse.move(0, 0)
await page.mouse.up()
```

- [mouse.click(x, y, **kwargs)](#mouse-click)
- [mouse.dblclick(x, y, **kwargs)](#mouse-dblclick)
- [mouse.down(**kwargs)](#mouse-down)
- [mouse.move(x, y, **kwargs)](#mouse-move)
- [mouse.up(**kwargs)](#mouse-up)
- [mouse.wheel(delta_x, delta_y)](#mouse-wheel)

## mouse.click(x, y, **kwargs)<a name="mouse-click">#</a>

- `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="mouse-click-option-x">#</a>
- `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="mouse-click-option-y">#</a>
- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="mouse-click-option-button">#</a>
- `click_count` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> defaults to 1. See [UIEvent.detail](https://developer.mozilla.org/en-US/docs/Web/API/UIEvent/detail).<a name="mouse-click-option-click-count">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="mouse-click-option-delay">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="mouse-click-return">#</a>

Shortcut for [mouse.move(x, y, **kwargs)](#mouse-move), [mouse.down(**kwargs)](#mouse-down), [mouse.up(**kwargs)](#mouse-up).

## mouse.dblclick(x, y, **kwargs)<a name="mouse-dblclick">#</a>

- `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="mouse-dblclick-option-x">#</a>
- `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="mouse-dblclick-option-y">#</a>
- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="mouse-dblclick-option-button">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="mouse-dblclick-option-delay">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="mouse-dblclick-return">#</a>

Shortcut for [mouse.move(x, y, **kwargs)](#mouse-move), [mouse.down(**kwargs)](#mouse-down), [mouse.up(**kwargs)](#mouse-up), [mouse.down(**kwargs)](#mouse-down) and [mouse.up(**kwargs)](#mouse-up).

## mouse.down(**kwargs)<a name="mouse-down">#</a>

- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="mouse-down-option-button">#</a>
- `click_count` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> defaults to 1. See [UIEvent.detail](https://developer.mozilla.org/en-US/docs/Web/API/UIEvent/detail).<a name="mouse-down-option-click-count">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="mouse-down-return">#</a>

Dispatches a `mousedown` event.

## mouse.move(x, y, **kwargs)<a name="mouse-move">#</a>

- `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="mouse-move-option-x">#</a>
- `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="mouse-move-option-y">#</a>
- `steps` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> defaults to 1. Sends intermediate `mousemove` events.<a name="mouse-move-option-steps">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="mouse-move-return">#</a>

Dispatches a `mousemove` event.

## mouse.up(**kwargs)<a name="mouse-up">#</a>

- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="mouse-up-option-button">#</a>
- `click_count` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> defaults to 1. See [UIEvent.detail](https://developer.mozilla.org/en-US/docs/Web/API/UIEvent/detail).<a name="mouse-up-option-click-count">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="mouse-up-return">#</a>

Dispatches a `mouseup` event.

## mouse.wheel(delta_x, delta_y)<a name="mouse-wheel">#</a>

- `delta_x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Pixels to scroll horizontally.<a name="mouse-wheel-option-delta-x">#</a>
- `delta_y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Pixels to scroll vertically.<a name="mouse-wheel-option-delta-y">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="mouse-wheel-return">#</a>

Dispatches a `wheel` event.

> NOTE
>
> Wheel events may cause scrolling if they are not handled, and this method does not wait for the scrolling to finish before returning.



# Page

- extends: [EventEmitter](https://pyee.readthedocs.io/en/latest/#pyee.BaseEventEmitter)

Page provides methods to interact with a single tab in a [Browser](#browser), or an [extension background page](https://developer.chrome.com/extensions/background_pages) in Chromium. One [Browser](#browser) instance might have multiple [Page](#page) instances.

This example creates a page, navigates it to a URL, and then saves a screenshot:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    webkit = playwright.webkit
    browser = webkit.launch()
    context = browser.new_context()
    page = context.new_page()
    page.goto("https://example.com")
    page.screenshot(path="screenshot.png")
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    webkit = playwright.webkit
    browser = await webkit.launch()
    context = await browser.new_context()
    page = await context.new_page()
    await page.goto("https://example.com")
    await page.screenshot(path="screenshot.png")
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

The Page class emits various events (described below) which can be handled using any of Node's native [`EventEmitter`](https://nodejs.org/api/events.html#events_class_eventemitter) methods, such as `on`, `once` or `removeListener`.

This example logs a message for a single page `load` event:

```python
page.once("load", lambda: print("page loaded!"))
```

To unsubscribe from events use the `removeListener` method:

```python
def log_request(intercepted_request):
    print("a request was made:", intercepted_request.url)
page.on("request", log_request)
# sometime later...
page.remove_listener("request", log_request)
```

- [page.on("close")](#page-event-close)
- [page.on("console")](#page-event-console)
- [page.on("crash")](#page-event-crash)
- [page.on("dialog")](#page-event-dialog)
- [page.on("domcontentloaded")](#page-event-dom-content-loaded)
- [page.on("download")](#page-event-download)
- [page.on("filechooser")](#page-event-file-chooser)
- [page.on("frameattached")](#page-event-frame-attached)
- [page.on("framedetached")](#page-event-frame-detached)
- [page.on("framenavigated")](#page-event-frame-navigated)
- [page.on("load")](#page-event-load)
- [page.on("pageerror")](#page-event-page-error)
- [page.on("popup")](#page-event-popup)
- [page.on("request")](#page-event-request)
- [page.on("requestfailed")](#page-event-request-failed)
- [page.on("requestfinished")](#page-event-request-finished)
- [page.on("response")](#page-event-response)
- [page.on("websocket")](#page-event-web-socket)
- [page.on("worker")](#page-event-worker)
- [page.add_init_script(**kwargs)](#page-add-init-script)
- [page.add_script_tag(**kwargs)](#page-add-script-tag)
- [page.add_style_tag(**kwargs)](#page-add-style-tag)
- [page.bring_to_front()](#page-bring-to-front)
- [page.check(selector, **kwargs)](#page-check)
- [page.click(selector, **kwargs)](#page-click)
- [page.close(**kwargs)](#page-close)
- [page.content()](#page-content)
- [page.context](#page-context)
- [page.dblclick(selector, **kwargs)](#page-dblclick)
- [page.dispatch_event(selector, type, **kwargs)](#page-dispatch-event)
- [page.drag_and_drop(source, target, **kwargs)](#page-drag-and-drop)
- [page.emulate_media(**kwargs)](#page-emulate-media)
- [page.eval_on_selector(selector, expression, **kwargs)](#page-eval-on-selector)
- [page.eval_on_selector_all(selector, expression, **kwargs)](#page-eval-on-selector-all)
- [page.evaluate(expression, **kwargs)](#page-evaluate)
- [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle)
- [page.expect_console_message(**kwargs)](#page-wait-for-console-message)
- [page.expect_download(**kwargs)](#page-wait-for-download)
- [page.expect_event(event, **kwargs)](#page-wait-for-event)
- [page.expect_file_chooser(**kwargs)](#page-wait-for-file-chooser)
- [page.expect_navigation(**kwargs)](#page-wait-for-navigation)
- [page.expect_popup(**kwargs)](#page-wait-for-popup)
- [page.expect_request(url_or_predicate, **kwargs)](#page-wait-for-request)
- [page.expect_request_finished(**kwargs)](#page-wait-for-request-finished)
- [page.expect_response(url_or_predicate, **kwargs)](#page-wait-for-response)
- [page.expect_websocket(**kwargs)](#page-wait-for-web-socket)
- [page.expect_worker(**kwargs)](#page-wait-for-worker)
- [page.expose_binding(name, callback, **kwargs)](#page-expose-binding)
- [page.expose_function(name, callback)](#page-expose-function)
- [page.fill(selector, value, **kwargs)](#page-fill)
- [page.focus(selector, **kwargs)](#page-focus)
- [page.frame(**kwargs)](#page-frame)
- [page.frame_locator(selector)](#page-frame-locator)
- [page.frames](#page-frames)
- [page.get_attribute(selector, name, **kwargs)](#page-get-attribute)
- [page.go_back(**kwargs)](#page-go-back)
- [page.go_forward(**kwargs)](#page-go-forward)
- [page.goto(url, **kwargs)](#page-goto)
- [page.hover(selector, **kwargs)](#page-hover)
- [page.inner_html(selector, **kwargs)](#page-inner-html)
- [page.inner_text(selector, **kwargs)](#page-inner-text)
- [page.input_value(selector, **kwargs)](#page-input-value)
- [page.is_checked(selector, **kwargs)](#page-is-checked)
- [page.is_closed()](#page-is-closed)
- [page.is_disabled(selector, **kwargs)](#page-is-disabled)
- [page.is_editable(selector, **kwargs)](#page-is-editable)
- [page.is_enabled(selector, **kwargs)](#page-is-enabled)
- [page.is_hidden(selector, **kwargs)](#page-is-hidden)
- [page.is_visible(selector, **kwargs)](#page-is-visible)
- [page.locator(selector, **kwargs)](#page-locator)
- [page.main_frame](#page-main-frame)
- [page.opener()](#page-opener)
- [page.pause()](#page-pause)
- [page.pdf(**kwargs)](#page-pdf)
- [page.press(selector, key, **kwargs)](#page-press)
- [page.query_selector(selector, **kwargs)](#page-query-selector)
- [page.query_selector_all(selector)](#page-query-selector-all)
- [page.reload(**kwargs)](#page-reload)
- [page.route(url, handler, **kwargs)](#page-route)
- [page.screenshot(**kwargs)](#page-screenshot)
- [page.select_option(selector, **kwargs)](#page-select-option)
- [page.set_checked(selector, checked, **kwargs)](#page-set-checked)
- [page.set_content(html, **kwargs)](#page-set-content)
- [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout)
- [page.set_default_timeout(timeout)](#page-set-default-timeout)
- [page.set_extra_http_headers(headers)](#page-set-extra-http-headers)
- [page.set_input_files(selector, files, **kwargs)](#page-set-input-files)
- [page.set_viewport_size(viewport_size)](#page-set-viewport-size)
- [page.tap(selector, **kwargs)](#page-tap)
- [page.text_content(selector, **kwargs)](#page-text-content)
- [page.title()](#page-title)
- [page.type(selector, text, **kwargs)](#page-type)
- [page.uncheck(selector, **kwargs)](#page-uncheck)
- [page.unroute(url, **kwargs)](#page-unroute)
- [page.url](#page-url)
- [page.video](#page-video)
- [page.viewport_size](#page-viewport-size)
- [page.wait_for_event(event, **kwargs)](#page-wait-for-event-2)
- [page.wait_for_function(expression, **kwargs)](#page-wait-for-function)
- [page.wait_for_load_state(**kwargs)](#page-wait-for-load-state)
- [page.wait_for_selector(selector, **kwargs)](#page-wait-for-selector)
- [page.wait_for_timeout(timeout)](#page-wait-for-timeout)
- [page.wait_for_url(url, **kwargs)](#page-wait-for-url)
- [page.workers](#page-workers)
- [page.accessibility](#page-accessibility)
- [page.keyboard](#page-keyboard)
- [page.mouse](#page-mouse)
- [page.request](#page-request)
- [page.touchscreen](#page-touchscreen)

## page.on("close")<a name="page-event-close">#</a>

- type: \<[Page](#page)>

Emitted when the page closes.

## page.on("console")<a name="page-event-console">#</a>

- type: \<[ConsoleMessage](#consolemessage)>

Emitted when JavaScript within the page calls one of console API methods, e.g. `console.log` or `console.dir`. Also emitted if the page throws an error or a warning.

The arguments passed into `console.log` appear as arguments on the event handler.

An example of handling `console` event:

- Sync

```python
def print_args(msg):
    for arg in msg.args:
        print(arg.json_value())

page.on("console", print_args)
page.evaluate("console.log('hello', 5, {foo: 'bar'})")
```

- Async

```python
async def print_args(msg):
    values = []
    for arg in msg.args:
        values.append(await arg.json_value())
    print(values)

page.on("console", print_args)
await page.evaluate("console.log('hello', 5, {foo: 'bar'})")
```

## page.on("crash")<a name="page-event-crash">#</a>

- type: \<[Page](#page)>

Emitted when the page crashes. Browser pages might crash if they try to allocate too much memory. When the page crashes, ongoing and subsequent operations will throw.

The most common way to deal with crashes is to catch an exception:

- Sync

```python
try:
    # crash might happen during a click.
    page.click("button")
    # or while waiting for an event.
    page.wait_for_event("popup")
except Error as e:
    # when the page crashes, exception message contains "crash".
```

- Async

```python
try:
    # crash might happen during a click.
    await page.click("button")
    # or while waiting for an event.
    await page.wait_for_event("popup")
except Error as e:
    # when the page crashes, exception message contains "crash".
```

## page.on("dialog")<a name="page-event-dialog">#</a>

- type: \<[Dialog](#dialog)>

Emitted when a JavaScript dialog appears, such as `alert`, `prompt`, `confirm` or `beforeunload`. Listener **must** either [dialog.accept(**kwargs)](#dialog-accept) or [dialog.dismiss()](#dialog-dismiss) the dialog - otherwise the page will [freeze](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop#never_blocking) waiting for the dialog, and actions like click will never finish.



> NOTE
>
> When no [page.on("dialog")](#page-event-dialog) listeners are present, all dialogs are automatically dismissed.

## page.on("domcontentloaded")<a name="page-event-dom-content-loaded">#</a>

- type: \<[Page](#page)>

Emitted when the JavaScript [`DOMContentLoaded`](https://developer.mozilla.org/en-US/docs/Web/Events/DOMContentLoaded) event is dispatched.

## page.on("download")<a name="page-event-download">#</a>

- type: \<[Download](#download)>

Emitted when attachment download started. User can access basic file operations on downloaded content via the passed [Download](#download) instance.

## page.on("filechooser")<a name="page-event-file-chooser">#</a>

- type: \<[FileChooser](#filechooser)>

Emitted when a file chooser is supposed to appear, such as after clicking the `\<input type=file>`. Playwright can respond to it via setting the input files using [file_chooser.set_files(files, **kwargs)](#file-chooser-set-files) that can be uploaded after that.

```python
page.on("filechooser", lambda file_chooser: file_chooser.set_files("/tmp/myfile.pdf"))
```

## page.on("frameattached")<a name="page-event-frame-attached">#</a>

- type: \<[Frame](#frame)>

Emitted when a frame is attached.

## page.on("framedetached")<a name="page-event-frame-detached">#</a>

- type: \<[Frame](#frame)>

Emitted when a frame is detached.

## page.on("framenavigated")<a name="page-event-frame-navigated">#</a>

- type: \<[Frame](#frame)>

Emitted when a frame is navigated to a new url.

## page.on("load")<a name="page-event-load">#</a>

- type: \<[Page](#page)>

Emitted when the JavaScript [`load`](https://developer.mozilla.org/en-US/docs/Web/Events/load) event is dispatched.

## page.on("pageerror")<a name="page-event-page-error">#</a>

- type: \<[Error](#error)>

Emitted when an uncaught exception happens within the page.

## page.on("popup")<a name="page-event-popup">#</a>

- type: \<[Page](#page)>

Emitted when the page opens a new tab or window. This event is emitted in addition to the [browser_context.on("page")](#browser-context-event-page), but only for popups relevant to this page.

The earliest moment that page is available is when it has navigated to the initial url. For example, when opening a popup with `window.open('http://example.com')`, this event will fire when the network request to "[http://example.com"](http://example.com"/) is done and its response has started loading in the popup.

- Sync

```python
with page.expect_event("popup") as page_info:
    page.evaluate("window.open('https://example.com')")
popup = page_info.value
print(popup.evaluate("location.href"))
```

- Async

```python
async with page.expect_event("popup") as page_info:
    page.evaluate("window.open('https://example.com')")
popup = await page_info.value
print(await popup.evaluate("location.href"))
```

> NOTE
>
> Use [page.wait_for_load_state(**kwargs)](#page-wait-for-load-state) to wait until the page gets to a particular state (you should not need it in most cases).

## page.on("request")<a name="page-event-request">#</a>

- type: \<[Request](#request)>

Emitted when a page issues a request. The [request](#request) object is read-only. In order to intercept and mutate requests, see [page.route(url, handler, **kwargs)](#page-route) or [browser_context.route(url, handler, **kwargs)](#browser-context-route).

## page.on("requestfailed")<a name="page-event-request-failed">#</a>

- type: \<[Request](#request)>

Emitted when a request fails, for example by timing out.

> NOTE
>
> HTTP Error responses, such as 404 or 503, are still successful responses from HTTP standpoint, so request will complete with [page.on("requestfinished")](#page-event-request-finished) event and not with [page.on("requestfailed")](#page-event-request-failed). A request will only be considered failed when the client cannot get an HTTP response from the server, e.g. due to network error net::ERR_FAILED.

## page.on("requestfinished")<a name="page-event-request-finished">#</a>

- type: \<[Request](#request)>

Emitted when a request finishes successfully after downloading the response body. For a successful response, the sequence of events is `request`, `response` and `requestfinished`.

## page.on("response")<a name="page-event-response">#</a>

- type: \<[Response](#response)>

Emitted when [response](#response) status and headers are received for a request. For a successful response, the sequence of events is `request`, `response` and `requestfinished`.

## page.on("websocket")<a name="page-event-web-socket">#</a>

- type: \<[WebSocket](#websocket)>

Emitted when [WebSocket](#websocket) request is sent.

## page.on("worker")<a name="page-event-worker">#</a>

- type: \<[Worker](#worker)>

Emitted when a dedicated [WebWorker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API) is spawned by the page.

## page.add_init_script(**kwargs)<a name="page-add-init-script">#</a>

- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to the JavaScript file. If `path` is a relative path, then it is resolved relative to the current working directory. Optional.<a name="page-add-init-script-option-path">#</a>
- `script` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Script to be evaluated in all pages in the browser context. Optional.<a name="page-add-init-script-option-script">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-add-init-script-return">#</a>

Adds a script which would be evaluated in one of the following scenarios:

- Whenever the page is navigated.
- Whenever the child frame is attached or navigated. In this case, the script is evaluated in the context of the newly attached frame.

The script is evaluated after the document was created but before any of its scripts were run. This is useful to amend the JavaScript environment, e.g. to seed `Math.random`.

An example of overriding `Math.random` before the page loads:

```js
// preload.js
Math.random = () => 42;
```

- Sync

```python
# in your playwright script, assuming the preload.js file is in same directory
page.add_init_script(path="./preload.js"
```

- Async

```python
# in your playwright script, assuming the preload.js file is in same directory
await page.add_init_script(path="./preload.js")
```

> NOTE
>
> The order of evaluation of multiple scripts installed via [browser_context.add_init_script(**kwargs)](#browser-context-add-init-script) and [page.add_init_script(**kwargs)](#page-add-init-script) is not defined.

## page.add_script_tag(**kwargs)<a name="page-add-script-tag">#</a>

- `content` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Raw JavaScript content to be injected into frame.<a name="page-add-script-tag-option-content">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to the JavaScript file to be injected into frame. If `path` is a relative path, then it is resolved relative to the current working directory.<a name="page-add-script-tag-option-path">#</a>
- `type` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Script type. Use 'module' in order to load a Javascript ES6 module. See [script](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) for more details.<a name="page-add-script-tag-option-type">#</a>
- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> URL of a script to be added.<a name="page-add-script-tag-option-url">#</a>
- returns: \<[ElementHandle](#elementhandle)><a name="page-add-script-tag-return">#</a>

Adds a `\<script>` tag into the page with the desired url or content. Returns the added tag when the script's onload fires or when the script content was injected into frame.

Shortcut for main frame's [frame.add_script_tag(**kwargs)](#frame-add-script-tag).

## page.add_style_tag(**kwargs)<a name="page-add-style-tag">#</a>

- `content` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Raw CSS content to be injected into frame.<a name="page-add-style-tag-option-content">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to the CSS file to be injected into frame. If `path` is a relative path, then it is resolved relative to the current working directory.<a name="page-add-style-tag-option-path">#</a>
- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> URL of the `\<link>` tag.<a name="page-add-style-tag-option-url">#</a>
- returns: \<[ElementHandle](#elementhandle)><a name="page-add-style-tag-return">#</a>

Adds a `\<link rel="stylesheet">` tag into the page with the desired url or a `\<style type="text/css">` tag with the content. Returns the added tag when the stylesheet's onload fires or when the CSS content was injected into frame.

Shortcut for main frame's [frame.add_style_tag(**kwargs)](#frame-add-style-tag).

## page.bring_to_front()<a name="page-bring-to-front">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-bring-to-front-return">#</a>

Brings page to front (activates tab).

## page.check(selector, **kwargs)<a name="page-check">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-check-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-check-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-check-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="page-check-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-check-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-check-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="page-check-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-check-return">#</a>

This method checks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Ensure that matched element is a checkbox or a radio input. If not, this method throws. If the element is already checked, this method returns immediately.
3. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
4. Scroll the element into view if needed.
5. Use [page.mouse](#page-mouse) to click in the center of the element.
6. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
7. Ensure that the element is now checked. If not, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

Shortcut for main frame's [frame.check(selector, **kwargs)](#frame-check).

## page.click(selector, **kwargs)<a name="page-click">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-click-option-selector">#</a>
- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="page-click-option-button">#</a>
- `click_count` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> defaults to 1. See [UIEvent.detail](https://developer.mozilla.org/en-US/docs/Web/API/UIEvent/detail).<a name="page-click-option-click-count">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="page-click-option-delay">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-click-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="page-click-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-click-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="page-click-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-click-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-click-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="page-click-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-click-return">#</a>

This method clicks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to click in the center of the element, or the specified `position`.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

Shortcut for main frame's [frame.click(selector, **kwargs)](#frame-click).

## page.close(**kwargs)<a name="page-close">#</a>

- `run_before_unload` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Defaults to `false`. Whether to run the [before unload](https://developer.mozilla.org/en-US/docs/Web/Events/beforeunload) page handlers.<a name="page-close-option-run-before-unload">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-close-return">#</a>

If `run_before_unload` is `false`, does not run any unload handlers and waits for the page to be closed. If `run_before_unload` is `true` the method will run unload handlers, but will **not** wait for the page to close.

By default, `page.close()` **does not** run `beforeunload` handlers.

> NOTE
>
> if `run_before_unload` is passed as true, a `beforeunload` dialog might be summoned and should be handled manually via [page.on("dialog")](#page-event-dialog) event.

## page.content()<a name="page-content">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-content-return">#</a>

Gets the full HTML contents of the page, including the doctype.

## page.context<a name="page-context">#</a>

- returns: \<[BrowserContext](#browsercontext)><a name="page-context-return">#</a>

Get the browser context that the page belongs to.

## page.dblclick(selector, **kwargs)<a name="page-dblclick">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-dblclick-option-selector">#</a>
- `button` \<"left"|"right"|"middle"> Defaults to `left`.<a name="page-dblclick-option-button">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `mousedown` and `mouseup` in milliseconds. Defaults to 0.<a name="page-dblclick-option-delay">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-dblclick-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="page-dblclick-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-dblclick-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="page-dblclick-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-dblclick-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-dblclick-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="page-dblclick-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-dblclick-return">#</a>

This method double clicks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to double click in the center of the element, or the specified `position`.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set. Note that if the first click of the `dblclick()` triggers a navigation event, this method will throw.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

> NOTE
>
> `page.dblclick()` dispatches two `click` events and a single `dblclick` event.

Shortcut for main frame's [frame.dblclick(selector, **kwargs)](#frame-dblclick).

## page.dispatch_event(selector, type, **kwargs)<a name="page-dispatch-event">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-dispatch-event-option-selector">#</a>
- `type` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> DOM event type: `"click"`, `"dragstart"`, etc.<a name="page-dispatch-event-option-type">#</a>
- `event_init` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional event-specific initialization properties.<a name="page-dispatch-event-option-event-init">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-dispatch-event-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-dispatch-event-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-dispatch-event-return">#</a>

The snippet below dispatches the `click` event on the element. Regardless of the visibility state of the element, `click` is dispatched. This is equivalent to calling [element.click()](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/click).

- Sync

```python
page.dispatch_event("button#submit", "click")
```

- Async

```python
await page.dispatch_event("button#submit", "click")
```

Under the hood, it creates an instance of an event based on the given `type`, initializes it with `event_init` properties and dispatches it on the element. Events are `composed`, `cancelable` and bubble by default.

Since `event_init` is event-specific, please refer to the events documentation for the lists of initial properties:

- [DragEvent](https://developer.mozilla.org/en-US/docs/Web/API/DragEvent/DragEvent)
- [FocusEvent](https://developer.mozilla.org/en-US/docs/Web/API/FocusEvent/FocusEvent)
- [KeyboardEvent](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/KeyboardEvent)
- [MouseEvent](https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent/MouseEvent)
- [PointerEvent](https://developer.mozilla.org/en-US/docs/Web/API/PointerEvent/PointerEvent)
- [TouchEvent](https://developer.mozilla.org/en-US/docs/Web/API/TouchEvent/TouchEvent)
- [Event](https://developer.mozilla.org/en-US/docs/Web/API/Event/Event)

You can also specify `JSHandle` as the property value if you want live objects to be passed into the event:

- Sync

```python
# note you can only create data_transfer in chromium and firefox
data_transfer = page.evaluate_handle("new DataTransfer()")
page.dispatch_event("#source", "dragstart", { "dataTransfer": data_transfer })
```

- Async

```python
# note you can only create data_transfer in chromium and firefox
data_transfer = await page.evaluate_handle("new DataTransfer()")
await page.dispatch_event("#source", "dragstart", { "dataTransfer": data_transfer })
```

## page.drag_and_drop(source, target, **kwargs)<a name="page-drag-and-drop">#</a>

- `source` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-drag-and-drop-option-source">#</a>
- `target` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-drag-and-drop-option-target">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-drag-and-drop-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-drag-and-drop-option-no-wait-after">#</a>
- `source_position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Clicks on the source element at this point relative to the top-left corner of the element's padding box. If not specified, some visible point of the element is used.<a name="page-drag-and-drop-option-source-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-drag-and-drop-option-strict">#</a>
- `target_position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Drops on the target element at this point relative to the top-left corner of the element's padding box. If not specified, some visible point of the element is used.<a name="page-drag-and-drop-option-target-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-drag-and-drop-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="page-drag-and-drop-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-drag-and-drop-return">#</a>

## page.emulate_media(**kwargs)<a name="page-emulate-media">#</a>

- `color_scheme` \<[NoneType](https://docs.python.org/3/library/constants.html#None)|"light"|"dark"|"no-preference"> Emulates `'prefers-colors-scheme'` media feature, supported values are `'light'`, `'dark'`, `'no-preference'`. Passing `null` disables color scheme emulation.<a name="page-emulate-media-option-color-scheme">#</a>

- `forced_colors` \<[NoneType](https://docs.python.org/3/library/constants.html#None)|"active"|"none"> Emulates `'forced-colors'` media feature, supported values are `'active'` and `'none'`. Passing `null` disables forced colors emulation.<a name="page-emulate-media-option-forced-colors">#</a>

    ##### NOTE

    It's not supported in WebKit, see [here](https://bugs.webkit.org/show_bug.cgi?id=225281) in their issue tracker.

- `media` \<[NoneType](https://docs.python.org/3/library/constants.html#None)|"screen"|"print"> Changes the CSS media type of the page. The only allowed values are `'screen'`, `'print'` and `null`. Passing `null` disables CSS media emulation.<a name="page-emulate-media-option-media">#</a>

- `reduced_motion` \<[NoneType](https://docs.python.org/3/library/constants.html#None)|"reduce"|"no-preference"> Emulates `'prefers-reduced-motion'` media feature, supported values are `'reduce'`, `'no-preference'`. Passing `null` disables reduced motion emulation.<a name="page-emulate-media-option-reduced-motion">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-emulate-media-return">#</a>

This method changes the `CSS media type` through the `media` argument, and/or the `'prefers-colors-scheme'` media feature, using the `colorScheme` argument.

- Sync

```python
page.evaluate("matchMedia('screen').matches")
# → True
page.evaluate("matchMedia('print').matches")
# → False

page.emulate_media(media="print")
page.evaluate("matchMedia('screen').matches")
# → False
page.evaluate("matchMedia('print').matches")
# → True

page.emulate_media()
page.evaluate("matchMedia('screen').matches")
# → True
page.evaluate("matchMedia('print').matches")
# → False
```

- Async

```python
await page.evaluate("matchMedia('screen').matches")
# → True
await page.evaluate("matchMedia('print').matches")
# → False

await page.emulate_media(media="print")
await page.evaluate("matchMedia('screen').matches")
# → False
await page.evaluate("matchMedia('print').matches")
# → True

await page.emulate_media()
await page.evaluate("matchMedia('screen').matches")
# → True
await page.evaluate("matchMedia('print').matches")
# → False
```

- Sync

```python
page.emulate_media(color_scheme="dark")
page.evaluate("matchMedia('(prefers-color-scheme: dark)').matches")
# → True
page.evaluate("matchMedia('(prefers-color-scheme: light)').matches")
# → False
page.evaluate("matchMedia('(prefers-color-scheme: no-preference)').matches")
```

- Async

```python
await page.emulate_media(color_scheme="dark")
await page.evaluate("matchMedia('(prefers-color-scheme: dark)').matches")
# → True
await page.evaluate("matchMedia('(prefers-color-scheme: light)').matches")
# → False
await page.evaluate("matchMedia('(prefers-color-scheme: no-preference)').matches")
# → False
```

## page.eval_on_selector(selector, expression, **kwargs)<a name="page-eval-on-selector">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-eval-on-selector-option-selector">#</a>
- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="page-eval-on-selector-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="page-eval-on-selector-option-arg">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-eval-on-selector-option-strict">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="page-eval-on-selector-return">#</a>

> CAUTION
>
> This method does not wait for the element to pass actionability checks and therefore can lead to the flaky tests. Use [locator.evaluate(expression, **kwargs)](#locator-evaluate), other [Locator](#locator) helper methods or web-first assertions instead.

The method finds an element matching the specified selector within the page and passes it as a first argument to `expression`. If no elements match the selector, the method throws an error. Returns the value of `expression`.

If `expression` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [page.eval_on_selector(selector, expression, **kwargs)](#page-eval-on-selector) would wait for the promise to resolve and return its value.

Examples:

- Sync

```python
search_value = page.eval_on_selector("#search", "el => el.value")
preload_href = page.eval_on_selector("link[rel=preload]", "el => el.href")
html = page.eval_on_selector(".main-container", "(e, suffix) => e.outer_html + suffix", "hello")
```

- Async

```python
search_value = await page.eval_on_selector("#search", "el => el.value")
preload_href = await page.eval_on_selector("link[rel=preload]", "el => el.href")
html = await page.eval_on_selector(".main-container", "(e, suffix) => e.outer_html + suffix", "hello")
```

Shortcut for main frame's [frame.eval_on_selector(selector, expression, **kwargs)](#frame-eval-on-selector).

## page.eval_on_selector_all(selector, expression, **kwargs)<a name="page-eval-on-selector-all">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-eval-on-selector-all-option-selector">#</a>
- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="page-eval-on-selector-all-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="page-eval-on-selector-all-option-arg">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="page-eval-on-selector-all-return">#</a>

> NOTE
>
> In most cases, [locator.evaluate_all(expression, **kwargs)](#locator-evaluate-all), other [Locator](#locator) helper methods and web-first assertions do a better job.

The method finds all elements matching the specified selector within the page and passes an array of matched elements as a first argument to `expression`. Returns the result of `expression` invocation.

If `expression` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [page.eval_on_selector_all(selector, expression, **kwargs)](#page-eval-on-selector-all) would wait for the promise to resolve and return its value.

Examples:

- Sync

```python
div_counts = page.eval_on_selector_all("div", "(divs, min) => divs.length >= min", 10)
```

- Async

```python
div_counts = await page.eval_on_selector_all("div", "(divs, min) => divs.length >= min", 10)
```

## page.evaluate(expression, **kwargs)<a name="page-evaluate">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="page-evaluate-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="page-evaluate-option-arg">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="page-evaluate-return">#</a>

Returns the value of the `expression` invocation.

If the function passed to the [page.evaluate(expression, **kwargs)](#page-evaluate) returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [page.evaluate(expression, **kwargs)](#page-evaluate) would wait for the promise to resolve and return its value.

If the function passed to the [page.evaluate(expression, **kwargs)](#page-evaluate) returns a non-[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description) value, then [page.evaluate(expression, **kwargs)](#page-evaluate) resolves to `undefined`. Playwright also supports transferring some additional values that are not serializable by `JSON`: `-0`, `NaN`, `Infinity`, `-Infinity`.

Passing argument to `expression`:

- Sync

```python
result = page.evaluate("([x, y]) => Promise.resolve(x * y)", [7, 8])
print(result) # prints "56"
```

- Async

```python
result = await page.evaluate("([x, y]) => Promise.resolve(x * y)", [7, 8])
print(result) # prints "56"
```

A string can also be passed in instead of a function:

- Sync

```python
print(page.evaluate("1 + 2")) # prints "3"
x = 10
print(page.evaluate(f"1 + {x}")) # prints "11"
```

- Async

```python
print(await page.evaluate("1 + 2")) # prints "3"
x = 10
print(await page.evaluate(f"1 + {x}")) # prints "11"
```

[ElementHandle](#elementhandle) instances can be passed as an argument to the [page.evaluate(expression, **kwargs)](#page-evaluate):

- Sync

```python
body_handle = page.evaluate("document.body")
html = page.evaluate("([body, suffix]) => body.innerHTML + suffix", [body_handle, "hello"])
body_handle.dispose()
```

- Async

```python
body_handle = await page.evaluate("document.body")
html = await page.evaluate("([body, suffix]) => body.innerHTML + suffix", [body_handle, "hello"])
await body_handle.dispose()
```

Shortcut for main frame's [frame.evaluate(expression, **kwargs)](#frame-evaluate).

## page.evaluate_handle(expression, **kwargs)<a name="page-evaluate-handle">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="page-evaluate-handle-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="page-evaluate-handle-option-arg">#</a>
- returns: \<[JSHandle](#jshandle)><a name="page-evaluate-handle-return">#</a>

Returns the value of the `expression` invocation as a [JSHandle](#jshandle).

The only difference between [page.evaluate(expression, **kwargs)](#page-evaluate) and [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle) is that [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle) returns [JSHandle](#jshandle).

If the function passed to the [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle) returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle) would wait for the promise to resolve and return its value.

- Sync

```python
a_window_handle = page.evaluate_handle("Promise.resolve(window)")
a_window_handle # handle for the window object.
```

- Async

```python
a_window_handle = await page.evaluate_handle("Promise.resolve(window)")
a_window_handle # handle for the window object.
```

A string can also be passed in instead of a function:

- Sync

```python
a_handle = page.evaluate_handle("document") # handle for the "document"
```

- Async

```python
a_handle = await page.evaluate_handle("document") # handle for the "document"
```

[JSHandle](#jshandle) instances can be passed as an argument to the [page.evaluate_handle(expression, **kwargs)](#page-evaluate-handle):

- Sync

```python
a_handle = page.evaluate_handle("document.body")
result_handle = page.evaluate_handle("body => body.innerHTML", a_handle)
print(result_handle.json_value())
result_handle.dispose()
```

- Async

```python
a_handle = await page.evaluate_handle("document.body")
result_handle = await page.evaluate_handle("body => body.innerHTML", a_handle)
print(await result_handle.json_value())
await result_handle.dispose()
```

## page.expect_console_message(**kwargs)<a name="page-wait-for-console-message">#</a>

- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[ConsoleMessage](#consolemessage)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Receives the [ConsoleMessage](#consolemessage) object and resolves to truthy value when the waiting should resolve.<a name="page-wait-for-console-message-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-console-message-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[ConsoleMessage](#consolemessage)]><a name="page-wait-for-console-message-return">#</a>

Performs action and waits for a [ConsoleMessage](#consolemessage) to be logged by in the page. If predicate is provided, it passes [ConsoleMessage](#consolemessage) value into the `predicate` function and waits for `predicate(message)` to return a truthy value. Will throw an error if the page is closed before the [page.on("console")](#page-event-console) event is fired.

## page.expect_download(**kwargs)<a name="page-wait-for-download">#</a>

- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Download](#download)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Receives the [Download](#download) object and resolves to truthy value when the waiting should resolve.<a name="page-wait-for-download-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-download-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[Download](#download)]><a name="page-wait-for-download-return">#</a>

Performs action and waits for a new [Download](#download). If predicate is provided, it passes [Download](#download) value into the `predicate` function and waits for `predicate(download)` to return a truthy value. Will throw an error if the page is closed before the download event is fired.

## page.expect_event(event, **kwargs)<a name="page-wait-for-event">#</a>

- `event` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Event name, same one typically passed into `*.on(event)`.<a name="page-wait-for-event-option-event">#</a>
- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Receives the event data and resolves to truthy value when the waiting should resolve.<a name="page-wait-for-event-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-event-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)><a name="page-wait-for-event-return">#</a>

Waits for event to fire and passes its value into the predicate function. Returns when the predicate returns truthy value. Will throw an error if the page is closed before the event is fired. Returns the event data value.

- Sync

```python
with page.expect_event("framenavigated") as event_info:
    page.click("button")
frame = event_info.value
```

- Async

```python
async with page.expect_event("framenavigated") as event_info:
    await page.click("button")
frame = await event_info.value
```

## page.expect_file_chooser(**kwargs)<a name="page-wait-for-file-chooser">#</a>

- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[FileChooser](#filechooser)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Receives the [FileChooser](#filechooser) object and resolves to truthy value when the waiting should resolve.<a name="page-wait-for-file-chooser-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-file-chooser-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[FileChooser](#filechooser)]><a name="page-wait-for-file-chooser-return">#</a>

Performs action and waits for a new [FileChooser](#filechooser) to be created. If predicate is provided, it passes [FileChooser](#filechooser) value into the `predicate` function and waits for `predicate(fileChooser)` to return a truthy value. Will throw an error if the page is closed before the file chooser is opened.

## page.expect_navigation(**kwargs)<a name="page-wait-for-navigation">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-wait-for-navigation-option-timeout">#</a>
- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[URL](https://en.wikipedia.org/wiki/URL)]:[bool](https://docs.python.org/3/library/stdtypes.html)> A glob pattern, regex pattern or predicate receiving [URL](https://en.wikipedia.org/wiki/URL) to match while waiting for the navigation. Note that if the parameter is a string without wilcard characters, the method will wait for navigation to URL that is exactly equal to the string.<a name="page-wait-for-navigation-option-url">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="page-wait-for-navigation-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[Response](#response)]><a name="page-wait-for-navigation-return">#</a>

Waits for the main frame navigation and returns the main resource response. In case of multiple redirects, the navigation will resolve with the response of the last redirect. In case of navigation to a different anchor or navigation due to History API usage, the navigation will resolve with `null`.

This resolves when the page navigates to a new URL or reloads. It is useful for when you run code which will indirectly cause the page to navigate. e.g. The click target has an `onclick` handler that triggers navigation from a `setTimeout`. Consider this example:

- Sync

```python
with page.expect_navigation():
    page.click("a.delayed-navigation") # clicking the link will indirectly cause a navigation
# Resolves after navigation has finished
```

- Async

```python
async with page.expect_navigation():
    await page.click("a.delayed-navigation") # clicking the link will indirectly cause a navigation
# Resolves after navigation has finished
```

> NOTE
>
> Usage of the [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) to change the URL is considered a navigation.

Shortcut for main frame's [frame.expect_navigation(**kwargs)](#frame-wait-for-navigation).

## page.expect_popup(**kwargs)<a name="page-wait-for-popup">#</a>

- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Page](#page)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Receives the [Page](#page) object and resolves to truthy value when the waiting should resolve.<a name="page-wait-for-popup-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-popup-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[Page](#page)]><a name="page-wait-for-popup-return">#</a>

Performs action and waits for a popup [Page](#page). If predicate is provided, it passes [Popup] value into the `predicate` function and waits for `predicate(page)` to return a truthy value. Will throw an error if the page is closed before the popup event is fired.

## page.expect_request(url_or_predicate, **kwargs)<a name="page-wait-for-request">#</a>

- `url_or_predicate` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Request](#request)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Request URL string, regex or predicate receiving [Request](#request) object. When a `base_url` via the context options was provided and the passed URL is a path, it gets merged via the [`new URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) constructor.<a name="page-wait-for-request-option-url-or-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum wait time in milliseconds, defaults to 30 seconds, pass `0` to disable the timeout. The default value can be changed by using the [page.set_default_timeout(timeout)](#page-set-default-timeout) method.<a name="page-wait-for-request-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[Request](#request)]><a name="page-wait-for-request-return">#</a>

Waits for the matching request and returns it. See [waiting for event](https://playwright.dev/python/docs/events#waiting-for-event) for more details about events.

- Sync

```python
with page.expect_request("http://example.com/resource") as first:
    page.click('button')
first_request = first.value

# or with a lambda
with page.expect_request(lambda request: request.url == "http://example.com" and request.method == "get") as second:
    page.click('img')
second_request = second.value
```

- Async

```python
async with page.expect_request("http://example.com/resource") as first:
    await page.click('button')
first_request = await first.value

# or with a lambda
async with page.expect_request(lambda request: request.url == "http://example.com" and request.method == "get") as second:
    await page.click('img')
second_request = await second.value
```

## page.expect_request_finished(**kwargs)<a name="page-wait-for-request-finished">#</a>

- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Request](#request)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Receives the [Request](#request) object and resolves to truthy value when the waiting should resolve.<a name="page-wait-for-request-finished-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-request-finished-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[Request](#request)]><a name="page-wait-for-request-finished-return">#</a>

Performs action and waits for a [Request](#request) to finish loading. If predicate is provided, it passes [Request](#request) value into the `predicate` function and waits for `predicate(request)` to return a truthy value. Will throw an error if the page is closed before the [page.on("requestfinished")](#page-event-request-finished) event is fired.

## page.expect_response(url_or_predicate, **kwargs)<a name="page-wait-for-response">#</a>

- `url_or_predicate` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Response](#response)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Request URL string, regex or predicate receiving [Response](#response) object. When a `base_url` via the context options was provided and the passed URL is a path, it gets merged via the [`new URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) constructor.<a name="page-wait-for-response-option-url-or-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum wait time in milliseconds, defaults to 30 seconds, pass `0` to disable the timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-wait-for-response-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[Response](#response)]><a name="page-wait-for-response-return">#</a>

Returns the matched response. See [waiting for event](https://playwright.dev/python/docs/events#waiting-for-event) for more details about events.

- Sync

```python
with page.expect_response("https://example.com/resource") as response_info:
    page.click("input")
response = response_info.value
return response.ok

# or with a lambda
with page.expect_response(lambda response: response.url == "https://example.com" and response.status == 200) as response_info:
    page.click("input")
response = response_info.value
return response.ok
```

- Async

```python
async with page.expect_response("https://example.com/resource") as response_info:
    await page.click("input")
response = await response_info.value
return response.ok

# or with a lambda
async with page.expect_response(lambda response: response.url == "https://example.com" and response.status == 200) as response_info:
    await page.click("input")
response = await response_info.value
return response.ok
```

## page.expect_websocket(**kwargs)<a name="page-wait-for-web-socket">#</a>

- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[WebSocket](#websocket)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Receives the [WebSocket](#websocket) object and resolves to truthy value when the waiting should resolve.<a name="page-wait-for-web-socket-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-web-socket-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[WebSocket](#websocket)]><a name="page-wait-for-web-socket-return">#</a>

Performs action and waits for a new [WebSocket](#websocket). If predicate is provided, it passes [WebSocket](#websocket) value into the `predicate` function and waits for `predicate(webSocket)` to return a truthy value. Will throw an error if the page is closed before the WebSocket event is fired.

## page.expect_worker(**kwargs)<a name="page-wait-for-worker">#</a>

- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Worker](#worker)]:[bool](https://docs.python.org/3/library/stdtypes.html)> Receives the [Worker](#worker) object and resolves to truthy value when the waiting should resolve.<a name="page-wait-for-worker-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-worker-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)[[Worker](#worker)]><a name="page-wait-for-worker-return">#</a>

Performs action and waits for a new [Worker](#worker). If predicate is provided, it passes [Worker](#worker) value into the `predicate` function and waits for `predicate(worker)` to return a truthy value. Will throw an error if the page is closed before the worker event is fired.

## page.expose_binding(name, callback, **kwargs)<a name="page-expose-binding">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the function on the window object.<a name="page-expose-binding-option-name">#</a>
- `callback` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Callback function that will be called in the Playwright's context.<a name="page-expose-binding-option-callback">#</a>
- `handle` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to pass the argument as a handle, instead of passing by value. When passing a handle, only one argument is supported. When passing by value, multiple arguments are supported.<a name="page-expose-binding-option-handle">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-expose-binding-return">#</a>

The method adds a function called `name` on the `window` object of every frame in this page. When called, the function executes `callback` and returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which resolves to the return value of `callback`. If the `callback` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), it will be awaited.

The first argument of the `callback` function contains information about the caller: `{ browserContext: BrowserContext, page: Page, frame: Frame }`.

See [browser_context.expose_binding(name, callback, **kwargs)](#browser-context-expose-binding) for the context-wide version.

> NOTE
>
> Functions installed via [page.expose_binding(name, callback, **kwargs)](#page-expose-binding) survive navigations.

An example of exposing page URL to all frames in a page:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    webkit = playwright.webkit
    browser = webkit.launch(headless=false)
    context = browser.new_context()
    page = context.new_page()
    page.expose_binding("pageURL", lambda source: source["page"].url)
    page.set_content("""
    \<script>
      async function onClick() {
        document.querySelector('div').textContent = await window.pageURL();
      }
    \</script>
    \<button onclick="onClick()">Click me\</button>
    \<div>\</div>
    """)
    page.click("button")

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    webkit = playwright.webkit
    browser = await webkit.launch(headless=false)
    context = await browser.new_context()
    page = await context.new_page()
    await page.expose_binding("pageURL", lambda source: source["page"].url)
    await page.set_content("""
    \<script>
      async function onClick() {
        document.querySelector('div').textContent = await window.pageURL();
      }
    \</script>
    \<button onclick="onClick()">Click me\</button>
    \<div>\</div>
    """)
    await page.click("button")

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

An example of passing an element handle:

- Sync

```python
def print(source, element):
    print(element.text_content())

page.expose_binding("clicked", print, handle=true)
page.set_content("""
  \<script>
    document.addEventListener('click', event => window.clicked(event.target));
  \</script>
  \<div>Click me\</div>
  \<div>Or click me\</div>
""")
```

- Async

```python
async def print(source, element):
    print(await element.text_content())

await page.expose_binding("clicked", print, handle=true)
await page.set_content("""
  \<script>
    document.addEventListener('click', event => window.clicked(event.target));
  \</script>
  \<div>Click me\</div>
  \<div>Or click me\</div>
""")
```

## page.expose_function(name, callback)<a name="page-expose-function">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the function on the window object<a name="page-expose-function-option-name">#</a>
- `callback` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Callback function which will be called in Playwright's context.<a name="page-expose-function-option-callback">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-expose-function-return">#</a>

The method adds a function called `name` on the `window` object of every frame in the page. When called, the function executes `callback` and returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which resolves to the return value of `callback`.

If the `callback` returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), it will be awaited.

See [browser_context.expose_function(name, callback)](#browser-context-expose-function) for context-wide exposed function.

> NOTE
>
> Functions installed via [page.expose_function(name, callback)](#page-expose-function) survive navigations.

An example of adding a `sha256` function to the page:

- Sync

```python
import hashlib
from playwright.sync_api import sync_playwright

def sha256(text):
    m = hashlib.sha256()
    m.update(bytes(text, "utf8"))
    return m.hexdigest()


def run(playwright):
    webkit = playwright.webkit
    browser = webkit.launch(headless=False)
    page = browser.new_page()
    page.expose_function("sha256", sha256)
    page.set_content("""
        \<script>
          async function onClick() {
            document.querySelector('div').textContent = await window.sha256('PLAYWRIGHT');
          }
        \</script>
        \<button onclick="onClick()">Click me\</button>
        \<div>\</div>
    """)
    page.click("button")

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
import hashlib
from playwright.async_api import async_playwright

def sha256(text):
    m = hashlib.sha256()
    m.update(bytes(text, "utf8"))
    return m.hexdigest()


async def run(playwright):
    webkit = playwright.webkit
    browser = await webkit.launch(headless=False)
    page = await browser.new_page()
    await page.expose_function("sha256", sha256)
    await page.set_content("""
        \<script>
          async function onClick() {
            document.querySelector('div').textContent = await window.sha256('PLAYWRIGHT');
          }
        \</script>
        \<button onclick="onClick()">Click me\</button>
        \<div>\</div>
    """)
    await page.click("button")

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

## page.fill(selector, value, **kwargs)<a name="page-fill">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-fill-option-selector">#</a>
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Value to fill for the `\<input>`, `\<textarea>` or `[contenteditable]` element.<a name="page-fill-option-value">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-fill-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-fill-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-fill-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-fill-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-fill-return">#</a>

This method waits for an element matching `selector`, waits for [actionability](https://playwright.dev/python/docs/actionability) checks, focuses the element, fills it and triggers an `input` event after filling. Note that you can pass an empty string to clear the input field.

If the target element is not an `\<input>`, `\<textarea>` or `[contenteditable]` element, this method throws an error. However, if the element is inside the `\<label>` element that has an associated [control](https://developer.mozilla.org/en-US/docs/Web/API/HTMLLabelElement/control), the control will be filled instead.

To send fine-grained keyboard events, use [page.type(selector, text, **kwargs)](#page-type).

Shortcut for main frame's [frame.fill(selector, value, **kwargs)](#frame-fill).

## page.focus(selector, **kwargs)<a name="page-focus">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-focus-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-focus-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-focus-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-focus-return">#</a>

This method fetches an element with `selector` and focuses it. If there's no element matching `selector`, the method waits until a matching element appears in the DOM.

Shortcut for main frame's [frame.focus(selector, **kwargs)](#frame-focus).

## page.frame(**kwargs)<a name="page-frame">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Frame name specified in the `iframe`'s `name` attribute. Optional.<a name="page-frame-option-name">#</a>
- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[URL](https://en.wikipedia.org/wiki/URL)]:[bool](https://docs.python.org/3/library/stdtypes.html)> A glob pattern, regex pattern or predicate receiving frame's `url` as a [URL](https://en.wikipedia.org/wiki/URL) object. Optional.<a name="page-frame-option-url">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Frame](#frame)><a name="page-frame-return">#</a>

Returns frame matching the specified criteria. Either `name` or `url` must be specified.

```python
frame = page.frame(name="frame-name")
```

```python
frame = page.frame(url=r".*domain.*")
```

## page.frame_locator(selector)<a name="page-frame-locator">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to use when resolving DOM element. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-frame-locator-option-selector">#</a>
- returns: \<[FrameLocator](#framelocator)><a name="page-frame-locator-return">#</a>

When working with iframes, you can create a frame locator that will enter the iframe and allow selecting elements in that iframe. Following snippet locates element with text "Submit" in the iframe with id `my-frame`, like `\<iframe id="my-frame">`:

- Sync

```python
locator = page.frame_locator("#my-iframe").locator("text=Submit")
locator.click()
```

- Async

```python
locator = page.frame_locator("#my-iframe").locator("text=Submit")
await locator.click()
```

## page.frames<a name="page-frames">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Frame](#frame)]><a name="page-frames-return">#</a>

An array of all frames attached to the page.

## page.get_attribute(selector, name, **kwargs)<a name="page-get-attribute">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-get-attribute-option-selector">#</a>
- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Attribute name to get the value for.<a name="page-get-attribute-option-name">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-get-attribute-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-get-attribute-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-get-attribute-return">#</a>

Returns element attribute value.

## page.go_back(**kwargs)<a name="page-go-back">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-go-back-option-timeout">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="page-go-back-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Response](#response)><a name="page-go-back-return">#</a>

Returns the main resource response. In case of multiple redirects, the navigation will resolve with the response of the last redirect. If can not go back, returns `null`.

Navigate to the previous page in history.

## page.go_forward(**kwargs)<a name="page-go-forward">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-go-forward-option-timeout">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="page-go-forward-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Response](#response)><a name="page-go-forward-return">#</a>

Returns the main resource response. In case of multiple redirects, the navigation will resolve with the response of the last redirect. If can not go forward, returns `null`.

Navigate to the next page in history.

## page.goto(url, **kwargs)<a name="page-goto">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> URL to navigate page to. The url should include scheme, e.g. `https://`. When a `base_url` via the context options was provided and the passed URL is a path, it gets merged via the [`new URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) constructor.<a name="page-goto-option-url">#</a>
- `referer` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Referer header value. If provided it will take preference over the referer header value set by [page.set_extra_http_headers(headers)](#page-set-extra-http-headers).<a name="page-goto-option-referer">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-goto-option-timeout">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="page-goto-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Response](#response)><a name="page-goto-return">#</a>

Returns the main resource response. In case of multiple redirects, the navigation will resolve with the response of the last redirect.

The method will throw an error if:

- there's an SSL error (e.g. in case of self-signed certificates).
- target URL is invalid.
- the `timeout` is exceeded during navigation.
- the remote server does not respond or is unreachable.
- the main resource failed to load.

The method will not throw an error when any valid HTTP status code is returned by the remote server, including 404 "Not Found" and 500 "Internal Server Error". The status code for such responses can be retrieved by calling [response.status](#response-status).

> NOTE
>
> The method either throws an error or returns a main resource response. The only exceptions are navigation to `about:blank` or navigation to the same URL with a different hash, which would succeed and return `null`.

> NOTE
>
> Headless mode doesn't support navigation to a PDF document. See the [upstream issue](https://bugs.chromium.org/p/chromium/issues/detail?id=761295).

Shortcut for main frame's [frame.goto(url, **kwargs)](#frame-goto)

## page.hover(selector, **kwargs)<a name="page-hover">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-hover-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-hover-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="page-hover-option-modifiers">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="page-hover-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-hover-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-hover-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="page-hover-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-hover-return">#</a>

This method hovers over an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
3. Scroll the element into view if needed.
4. Use [page.mouse](#page-mouse) to hover over the center of the element, or the specified `position`.
5. Wait for initiated navigations to either succeed or fail, unless `noWaitAfter` option is set.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

Shortcut for main frame's [frame.hover(selector, **kwargs)](#frame-hover).

## page.inner_html(selector, **kwargs)<a name="page-inner-html">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-inner-html-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-inner-html-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-inner-html-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-inner-html-return">#</a>

Returns `element.innerHTML`.

## page.inner_text(selector, **kwargs)<a name="page-inner-text">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-inner-text-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-inner-text-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-inner-text-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-inner-text-return">#</a>

Returns `element.innerText`.

## page.input_value(selector, **kwargs)<a name="page-input-value">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-input-value-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-input-value-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-input-value-option-timeout">#</a>
- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-input-value-return">#</a>

Returns `input.value` for the selected `\<input>` or `\<textarea>` or `\<select>` element. Throws for non-input elements.

## page.is_checked(selector, **kwargs)<a name="page-is-checked">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-is-checked-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-is-checked-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-is-checked-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="page-is-checked-return">#</a>

Returns whether the element is checked. Throws if the element is not a checkbox or radio input.

## page.is_closed()<a name="page-is-closed">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="page-is-closed-return">#</a>

Indicates that the page has been closed.

## page.is_disabled(selector, **kwargs)<a name="page-is-disabled">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-is-disabled-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-is-disabled-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-is-disabled-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="page-is-disabled-return">#</a>

Returns whether the element is disabled, the opposite of [enabled](https://playwright.dev/python/docs/actionability#enabled).

## page.is_editable(selector, **kwargs)<a name="page-is-editable">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-is-editable-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-is-editable-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-is-editable-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="page-is-editable-return">#</a>

Returns whether the element is [editable](https://playwright.dev/python/docs/actionability#editable).

## page.is_enabled(selector, **kwargs)<a name="page-is-enabled">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-is-enabled-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-is-enabled-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-is-enabled-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="page-is-enabled-return">#</a>

Returns whether the element is [enabled](https://playwright.dev/python/docs/actionability#enabled).

## page.is_hidden(selector, **kwargs)<a name="page-is-hidden">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-is-hidden-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-is-hidden-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> **DEPRECATED** This option is ignored. [page.is_hidden(selector, **kwargs)](#page-is-hidden) does not wait for the element to become hidden and returns immediately.<a name="page-is-hidden-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="page-is-hidden-return">#</a>

Returns whether the element is hidden, the opposite of [visible](https://playwright.dev/python/docs/actionability#visible). `selector` that does not match any elements is considered hidden.

## page.is_visible(selector, **kwargs)<a name="page-is-visible">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-is-visible-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-is-visible-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> **DEPRECATED** This option is ignored. [page.is_visible(selector, **kwargs)](#page-is-visible) does not wait for the element to become visible and returns immediately.<a name="page-is-visible-option-timeout">#</a>
- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="page-is-visible-return">#</a>

Returns whether the element is [visible](https://playwright.dev/python/docs/actionability#visible). `selector` that does not match any elements is considered not visible.

## page.locator(selector, **kwargs)<a name="page-locator">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to use when resolving DOM element. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-locator-option-selector">#</a>

- `has` \<[Locator](#locator)> Matches elements containing an element that matches an inner locator. Inner locator is queried against the outer one. For example, `article` that has `text=Playwright` matches `\<article>\<div>Playwright\</div>\</article>`.<a name="page-locator-option-has">#</a>

    Note that outer and inner locators must belong to the same frame. Inner locator must not contain [FrameLocator](#framelocator)s.

- `has_text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)> Matches elements containing specified text somewhere inside, possibly in a child or a descendant element. For example, `"Playwright"` matches `\<article>\<div>Playwright\</div>\</article>`.<a name="page-locator-option-has-text">#</a>

- returns: \<[Locator](#locator)><a name="page-locator-return">#</a>

The method returns an element locator that can be used to perform actions on the page. Locator is resolved to the element immediately before performing an action, so a series of actions on the same locator can in fact be performed on different DOM elements. That would happen if the DOM structure between those actions has changed.

Shortcut for main frame's [frame.locator(selector, **kwargs)](#frame-locator).

## page.main_frame<a name="page-main-frame">#</a>

- returns: \<[Frame](#frame)><a name="page-main-frame-return">#</a>

The page's main frame. Page is guaranteed to have a main frame which persists during navigations.

## page.opener()<a name="page-opener">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Page](#page)><a name="page-opener-return">#</a>

Returns the opener for popup pages and `null` for others. If the opener has been closed already the returns `null`.

## page.pause()<a name="page-pause">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-pause-return">#</a>

Pauses script execution. Playwright will stop executing the script and wait for the user to either press 'Resume' button in the page overlay or to call `playwright.resume()` in the DevTools console.

User can inspect selectors or perform manual steps while paused. Resume will continue running the original script from the place it was paused.

> NOTE
>
> This method requires Playwright to be started in a headed mode, with a falsy `headless` value in the [browser_type.launch(**kwargs)](#browser-type-launch).

## page.pdf(**kwargs)<a name="page-pdf">#</a>

- `display_header_footer` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Display header and footer. Defaults to `false`.<a name="page-pdf-option-display-header-footer">#</a>
- `footer_template` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> HTML template for the print footer. Should use the same format as the `header_template`.<a name="page-pdf-option-footer-template">#</a>
- `format` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Paper format. If set, takes priority over `width` or `height` options. Defaults to 'Letter'.<a name="page-pdf-option-format">#</a>
- `header_template` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> HTML template for the print header. Should be valid HTML markup with following classes used to inject printing values into them:<a name="page-pdf-option-header-template">#</a>
    - `'date'` formatted print date
    - `'title'` document title
    - `'url'` document location
    - `'pageNumber'` current page number
    - `'totalPages'` total pages in the document
- `height` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Paper height, accepts values labeled with units.<a name="page-pdf-option-height">#</a>
- `landscape` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Paper orientation. Defaults to `false`.<a name="page-pdf-option-landscape">#</a>
- `margin` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> Paper margins, defaults to none.<a name="page-pdf-option-margin">#</a>
    - `top` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Top margin, accepts values labeled with units. Defaults to `0`.
    - `right` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Right margin, accepts values labeled with units. Defaults to `0`.
    - `bottom` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Bottom margin, accepts values labeled with units. Defaults to `0`.
    - `left` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Left margin, accepts values labeled with units. Defaults to `0`.
- `page_ranges` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Paper ranges to print, e.g., '1-5, 8, 11-13'. Defaults to the empty string, which means print all pages.<a name="page-pdf-option-page-ranges">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> The file path to save the PDF to. If `path` is a relative path, then it is resolved relative to the current working directory. If no path is provided, the PDF won't be saved to the disk.<a name="page-pdf-option-path">#</a>
- `prefer_css_page_size` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Give any CSS `@page` size declared in the page priority over what is declared in `width` and `height` or `format` options. Defaults to `false`, which will scale the content to fit the paper size.<a name="page-pdf-option-prefer-css-page-size">#</a>
- `print_background` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Print background graphics. Defaults to `false`.<a name="page-pdf-option-print-background">#</a>
- `scale` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Scale of the webpage rendering. Defaults to `1`. Scale amount must be between 0.1 and 2.<a name="page-pdf-option-scale">#</a>
- `width` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Paper width, accepts values labeled with units.<a name="page-pdf-option-width">#</a>
- returns: \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)><a name="page-pdf-return">#</a>

Returns the PDF buffer.

> NOTE
>
> Generating a pdf is currently only supported in Chromium headless.

`page.pdf()` generates a pdf of the page with `print` css media. To generate a pdf with `screen` media, call [page.emulate_media(**kwargs)](#page-emulate-media) before calling `page.pdf()`:

> NOTE
>
> By default, `page.pdf()` generates a pdf with modified colors for printing. Use the [`-webkit-print-color-adjust`](https://developer.mozilla.org/en-US/docs/Web/CSS/-webkit-print-color-adjust) property to force rendering of exact colors.

- Sync

```python
# generates a pdf with "screen" media type.
page.emulate_media(media="screen")
page.pdf(path="page.pdf")
```

- Async

```python
# generates a pdf with "screen" media type.
await page.emulate_media(media="screen")
await page.pdf(path="page.pdf")
```

The `width`, `height`, and `margin` options accept values labeled with units. Unlabeled values are treated as pixels.

A few examples:

- `page.pdf({width: 100})` - prints with width set to 100 pixels
- `page.pdf({width: '100px'})` - prints with width set to 100 pixels
- `page.pdf({width: '10cm'})` - prints with width set to 10 centimeters.

All possible units are:

- `px` - pixel
- `in` - inch
- `cm` - centimeter
- `mm` - millimeter

The `format` options are:

- `Letter`: 8.5in x 11in
- `Legal`: 8.5in x 14in
- `Tabloid`: 11in x 17in
- `Ledger`: 17in x 11in
- `A0`: 33.1in x 46.8in
- `A1`: 23.4in x 33.1in
- `A2`: 16.54in x 23.4in
- `A3`: 11.7in x 16.54in
- `A4`: 8.27in x 11.7in
- `A5`: 5.83in x 8.27in
- `A6`: 4.13in x 5.83in

> NOTE
>
> `header_template` and `footer_template` markup have the following limitations: > 1. Script tags inside templates are not evaluated. > 2. Page styles are not visible inside templates.

## page.press(selector, key, **kwargs)<a name="page-press">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-press-option-selector">#</a>
- `key` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the key to press or a character to generate, such as `ArrowLeft` or `a`.<a name="page-press-option-key">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between `keydown` and `keyup` in milliseconds. Defaults to 0.<a name="page-press-option-delay">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-press-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-press-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-press-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-press-return">#</a>

Focuses the element, and then uses [keyboard.down(key)](#keyboard-down) and [keyboard.up(key)](#keyboard-up).

`key` can specify the intended [keyboardEvent.key](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key) value or a single character to generate the text for. A superset of the `key` values can be found [here](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values). Examples of the keys are:

`F1` - `F12`, `Digit0`- `Digit9`, `KeyA`- `KeyZ`, `Backquote`, `Minus`, `Equal`, `Backslash`, `Backspace`, `Tab`, `Delete`, `Escape`, `ArrowDown`, `End`, `Enter`, `Home`, `Insert`, `PageDown`, `PageUp`, `ArrowRight`, `ArrowUp`, etc.

Following modification shortcuts are also supported: `Shift`, `Control`, `Alt`, `Meta`, `ShiftLeft`.

Holding down `Shift` will type the text that corresponds to the `key` in the upper case.

If `key` is a single character, it is case-sensitive, so the values `a` and `A` will generate different respective texts.

Shortcuts such as `key: "Control+o"` or `key: "Control+Shift+T"` are supported as well. When specified with the modifier, modifier is pressed and being held while the subsequent key is being pressed.

- Sync

```python
page = browser.new_page()
page.goto("https://keycode.info")
page.press("body", "A")
page.screenshot(path="a.png")
page.press("body", "ArrowLeft")
page.screenshot(path="arrow_left.png")
page.press("body", "Shift+O")
page.screenshot(path="o.png")
browser.close()
```

- Async

```python
page = await browser.new_page()
await page.goto("https://keycode.info")
await page.press("body", "A")
await page.screenshot(path="a.png")
await page.press("body", "ArrowLeft")
await page.screenshot(path="arrow_left.png")
await page.press("body", "Shift+O")
await page.screenshot(path="o.png")
await browser.close()
```

## page.query_selector(selector, **kwargs)<a name="page-query-selector">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-query-selector-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-query-selector-option-strict">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[ElementHandle](#elementhandle)><a name="page-query-selector-return">#</a>

> CAUTION
>
> The use of [ElementHandle](#elementhandle) is discouraged, use [Locator](#locator) objects and web-first assertions instead.

The method finds an element matching the specified selector within the page. If no elements match the selector, the return value resolves to `null`. To wait for an element on the page, use [locator.wait_for(**kwargs)](#locator-wait-for).

Shortcut for main frame's [frame.query_selector(selector, **kwargs)](#frame-query-selector).

## page.query_selector_all(selector)<a name="page-query-selector-all">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-query-selector-all-option-selector">#</a>
- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[ElementHandle](#elementhandle)]><a name="page-query-selector-all-return">#</a>

> CAUTION
>
> The use of [ElementHandle](#elementhandle) is discouraged, use [Locator](#locator) objects and web-first assertions instead.

The method finds all elements matching the specified selector within the page. If no elements match the selector, the return value resolves to `[]`.

Shortcut for main frame's [frame.query_selector_all(selector)](#frame-query-selector-all).

## page.reload(**kwargs)<a name="page-reload">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-reload-option-timeout">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="page-reload-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Response](#response)><a name="page-reload-return">#</a>

This method reloads the current page, in the same way as if the user had triggered a browser refresh. Returns the main resource response. In case of multiple redirects, the navigation will resolve with the response of the last redirect.

## page.route(url, handler, **kwargs)<a name="page-route">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[URL](https://en.wikipedia.org/wiki/URL)]:[bool](https://docs.python.org/3/library/stdtypes.html)> A glob pattern, regex pattern or predicate receiving [URL](https://en.wikipedia.org/wiki/URL) to match while routing. When a `base_url` via the context options was provided and the passed URL is a path, it gets merged via the [`new URL()`](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) constructor.<a name="page-route-option-url">#</a>
- `handler` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Route](#route), [Request](#request)]> handler function to route the request.<a name="page-route-option-handler">#</a>
- `times` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> How often a route should be used. By default it will be used every time.<a name="page-route-option-times">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-route-return">#</a>

Routing provides the capability to modify network requests that are made by a page.

Once routing is enabled, every request matching the url pattern will stall unless it's continued, fulfilled or aborted.

> NOTE
>
> The handler will only be called for the first url if the response is a redirect.

> NOTE
>
> [page.route(url, handler, **kwargs)](#page-route) will not intercept requests intercepted by Service Worker. See [this](https://github.com/microsoft/playwright/issues/1090) issue. We recommend disabling Service Workers when using request interception. Via `await context.addInitScript(() => delete window.navigator.serviceWorker);`

An example of a naive handler that aborts all image requests:

- Sync

```python
page = browser.new_page()
page.route("**/*.{png,jpg,jpeg}", lambda route: route.abort())
page.goto("https://example.com")
browser.close()
```

- Async

```python
page = await browser.new_page()
await page.route("**/*.{png,jpg,jpeg}", lambda route: route.abort())
await page.goto("https://example.com")
await browser.close()
```

or the same snippet using a regex pattern instead:

- Sync

```python
page = browser.new_page()
page.route(re.compile(r"(\.png$)|(\.jpg$)"), lambda route: route.abort())
page.goto("https://example.com")
browser.close()
```

- Async

```python
page = await browser.new_page()
await page.route(re.compile(r"(\.png$)|(\.jpg$)"), lambda route: route.abort())
await page.goto("https://example.com")
await browser.close()
```

It is possible to examine the request to decide the route action. For example, mocking all requests that contain some post data, and leaving all other requests as is:

- Sync

```python
def handle_route(route):
  if ("my-string" in route.request.post_data)
    route.fulfill(body="mocked-data")
  else
    route.continue_()
page.route("/api/**", handle_route)
```

- Async

```python
def handle_route(route):
  if ("my-string" in route.request.post_data)
    route.fulfill(body="mocked-data")
  else
    route.continue_()
await page.route("/api/**", handle_route)
```

Page routes take precedence over browser context routes (set up with [browser_context.route(url, handler, **kwargs)](https://playwright.dev/python/docs/api/class-browsercontext#browser-context-route)) when request matches both handlers.

To remove a route with its handler you can use [page.unroute(url, **kwargs)](#page-unroute).

> NOTE
>
> Enabling routing disables http cache.

## page.screenshot(**kwargs)<a name="page-screenshot">#</a>

- `animations` \<"disabled"> When set to `"disabled"`, stops CSS animations, CSS transitions and Web Animations. Animations get different treatment depending on their duration:<a name="page-screenshot-option-animations">#</a>
    - finite animations are fast-forwarded to completion, so they'll fire `transitionend` event.
    - infinite animations are canceled to initial state, and then played over after the screenshot.
- `clip` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> An object which specifies clipping of the resulting image. Should have the following fields:<a name="page-screenshot-option-clip">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> x-coordinate of top-left corner of clip area
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> y-coordinate of top-left corner of clip area
    - `width` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> width of clipping area
    - `height` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> height of clipping area
- `full_page` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, takes a screenshot of the full scrollable page, instead of the currently visible viewport. Defaults to `false`.<a name="page-screenshot-option-full-page">#</a>
- `mask` \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Locator](#locator)]> Specify locators that should be masked when the screenshot is taken. Masked elements will be overlayed with a pink box `#FF00FF` that completely covers its bounding box.<a name="page-screenshot-option-mask">#</a>
- `omit_background` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Hides default white background and allows capturing screenshots with transparency. Not applicable to `jpeg` images. Defaults to `false`.<a name="page-screenshot-option-omit-background">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> The file path to save the image to. The screenshot type will be inferred from file extension. If `path` is a relative path, then it is resolved relative to the current working directory. If no path is provided, the image won't be saved to the disk.<a name="page-screenshot-option-path">#</a>
- `quality` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> The quality of the image, between 0-100. Not applicable to `png` images.<a name="page-screenshot-option-quality">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-screenshot-option-timeout">#</a>
- `type` \<"png"|"jpeg"> Specify screenshot type, defaults to `png`.<a name="page-screenshot-option-type">#</a>
- returns: \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)><a name="page-screenshot-return">#</a>

Returns the buffer with the captured screenshot.

## page.select_option(selector, **kwargs)<a name="page-select-option">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-select-option-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-select-option-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-select-option-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-select-option-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-select-option-option-timeout">#</a>
- `element` \<[ElementHandle](#elementhandle)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[ElementHandle](#elementhandle)]> Option elements to select. Optional.<a name="page-select-option-option-element">#</a>
- `index` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)]> Options to select by index. Optional.<a name="page-select-option-option-index">#</a>
- `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Options to select by value. If the `\<select>` has the `multiple` attribute, all given options are selected, otherwise only the first option matching one of the passed options is selected. Optional.<a name="page-select-option-option-value">#</a>
- `label` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Options to select by label. If the `\<select>` has the `multiple` attribute, all given options are selected, otherwise only the first option matching one of the passed options is selected. Optional.<a name="page-select-option-option-label">#</a>
- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="page-select-option-return">#</a>

This method waits for an element matching `selector`, waits for [actionability](https://playwright.dev/python/docs/actionability) checks, waits until all specified options are present in the `\<select>` element and selects these options.

If the target element is not a `\<select>` element, this method throws an error. However, if the element is inside the `\<label>` element that has an associated [control](https://developer.mozilla.org/en-US/docs/Web/API/HTMLLabelElement/control), the control will be used instead.

Returns the array of option values that have been successfully selected.

Triggers a `change` and `input` event once all the provided options have been selected.

- Sync

```python
# single selection matching the value
page.select_option("select#colors", "blue")
# single selection matching both the label
page.select_option("select#colors", label="blue")
# multiple selection
page.select_option("select#colors", value=["red", "green", "blue"])
```

- Async

```python
# single selection matching the value
await page.select_option("select#colors", "blue")
# single selection matching the label
await page.select_option("select#colors", label="blue")
# multiple selection
await page.select_option("select#colors", value=["red", "green", "blue"])
```

Shortcut for main frame's [frame.select_option(selector, **kwargs)](#frame-select-option).

## page.set_checked(selector, checked, **kwargs)<a name="page-set-checked">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-set-checked-option-selector">#</a>
- `checked` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to check or uncheck the checkbox.<a name="page-set-checked-option-checked">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-set-checked-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-set-checked-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="page-set-checked-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-set-checked-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-set-checked-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="page-set-checked-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-set-checked-return">#</a>

This method checks or unchecks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Ensure that matched element is a checkbox or a radio input. If not, this method throws.
3. If the element already has the right checked state, this method returns immediately.
4. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
5. Scroll the element into view if needed.
6. Use [page.mouse](#page-mouse) to click in the center of the element.
7. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
8. Ensure that the element is now checked or unchecked. If not, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

Shortcut for main frame's [frame.set_checked(selector, checked, **kwargs)](#frame-set-checked).

## page.set_content(html, **kwargs)<a name="page-set-content">#</a>

- `html` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> HTML markup to assign to the page.<a name="page-set-content-option-html">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-set-content-option-timeout">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="page-set-content-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-set-content-return">#</a>

## page.set_default_navigation_timeout(timeout)<a name="page-set-default-navigation-timeout">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum navigation time in milliseconds<a name="page-set-default-navigation-timeout-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-set-default-navigation-timeout-return">#</a>

This setting will change the default maximum navigation time for the following methods and related shortcuts:

- [page.go_back(**kwargs)](#page-go-back)
- [page.go_forward(**kwargs)](#page-go-forward)
- [page.goto(url, **kwargs)](#page-goto)
- [page.reload(**kwargs)](#page-reload)
- [page.set_content(html, **kwargs)](#page-set-content)
- [page.expect_navigation(**kwargs)](#page-wait-for-navigation)
- [page.wait_for_url(url, **kwargs)](#page-wait-for-url)

> NOTE
>
> [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) takes priority over [page.set_default_timeout(timeout)](#page-set-default-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) and [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout).

## page.set_default_timeout(timeout)<a name="page-set-default-timeout">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds<a name="page-set-default-timeout-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-set-default-timeout-return">#</a>

This setting will change the default maximum time for all the methods accepting `timeout` option.

> NOTE
>
> [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) takes priority over [page.set_default_timeout(timeout)](#page-set-default-timeout).

## page.set_extra_http_headers(headers)<a name="page-set-extra-http-headers">#</a>

- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> An object containing additional HTTP headers to be sent with every request. All header values must be strings.<a name="page-set-extra-http-headers-option-headers">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-set-extra-http-headers-return">#</a>

The extra HTTP headers will be sent with every request the page initiates.

> NOTE
>
> [page.set_extra_http_headers(headers)](#page-set-extra-http-headers) does not guarantee the order of headers in the outgoing requests.

## page.set_input_files(selector, files, **kwargs)<a name="page-set-input-files">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-set-input-files-option-selector">#</a>
- `files` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]]|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)|[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="page-set-input-files-option-files">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File name
    - `mimeType` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> File type
    - `buffer` \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> File content
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-set-input-files-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-set-input-files-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-set-input-files-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-set-input-files-return">#</a>

This method expects `selector` to point to an [input element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input).

Sets the value of the file input to these file paths or files. If some of the `filePaths` are relative paths, then they are resolved relative to the the current working directory. For empty array, clears the selected files.

## page.set_viewport_size(viewport_size)<a name="page-set-viewport-size">#</a>

- `viewport_size` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="page-set-viewport-size-option-viewport-size">#</a>
    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page width in pixels.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page height in pixels.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-set-viewport-size-return">#</a>

In the case of multiple pages in a single browser, each page can have its own viewport size. However, [browser.new_context(**kwargs)](#browser-new-context) allows to set viewport size (and more) for all pages in the context at once.

[page.set_viewport_size(viewport_size)](#page-set-viewport-size) will resize the page. A lot of websites don't expect phones to change size, so you should set the viewport size before navigating to the page. [page.set_viewport_size(viewport_size)](#page-set-viewport-size) will also reset `screen` size, use [browser.new_context(**kwargs)](#browser-new-context) with `screen` and `viewport` parameters if you need better control of these properties.

- Sync

```python
page = browser.new_page()
page.set_viewport_size({"width": 640, "height": 480})
page.goto("https://example.com")
```

- Async

```python
page = await browser.new_page()
await page.set_viewport_size({"width": 640, "height": 480})
await page.goto("https://example.com")
```

## page.tap(selector, **kwargs)<a name="page-tap">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-tap-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-tap-option-force">#</a>
- `modifiers` \<[List](https://docs.python.org/3/library/typing.html#typing.List)["Alt"|"Control"|"Meta"|"Shift"]> Modifier keys to press. Ensures that only these modifiers are pressed during the operation, and then restores current modifiers back. If not specified, currently pressed modifiers are used.<a name="page-tap-option-modifiers">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-tap-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="page-tap-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-tap-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-tap-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="page-tap-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-tap-return">#</a>

This method taps an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
3. Scroll the element into view if needed.
4. Use [page.touchscreen](#page-touchscreen) to tap the center of the element, or the specified `position`.
5. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

> NOTE
>
> [page.tap(selector, **kwargs)](#page-tap) requires that the `has_touch` option of the browser context be set to true.

Shortcut for main frame's [frame.tap(selector, **kwargs)](#frame-tap).

## page.text_content(selector, **kwargs)<a name="page-text-content">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-text-content-option-selector">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-text-content-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-text-content-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-text-content-return">#</a>

Returns `element.textContent`.

## page.title()<a name="page-title">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-title-return">#</a>

Returns the page's title. Shortcut for main frame's [frame.title()](#frame-title).

## page.type(selector, text, **kwargs)<a name="page-type">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-type-option-selector">#</a>
- `text` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A text to type into a focused element.<a name="page-type-option-text">#</a>
- `delay` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time to wait between key presses in milliseconds. Defaults to 0.<a name="page-type-option-delay">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-type-option-no-wait-after">#</a>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-type-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-type-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-type-return">#</a>

Sends a `keydown`, `keypress`/`input`, and `keyup` event for each character in the text. `page.type` can be used to send fine-grained keyboard events. To fill values in form fields, use [page.fill(selector, value, **kwargs)](#page-fill).

To press a special key, like `Control` or `ArrowDown`, use [keyboard.press(key, **kwargs)](#keyboard-press).

- Sync

```python
page.type("#mytextarea", "hello") # types instantly
page.type("#mytextarea", "world", delay=100) # types slower, like a user
```

- Async

```python
await page.type("#mytextarea", "hello") # types instantly
await page.type("#mytextarea", "world", delay=100) # types slower, like a user
```

Shortcut for main frame's [frame.type(selector, text, **kwargs)](#frame-type).

## page.uncheck(selector, **kwargs)<a name="page-uncheck">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to search for an element. If there are multiple elements satisfying the selector, the first will be used. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-uncheck-option-selector">#</a>
- `force` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to bypass the [actionability](https://playwright.dev/python/docs/actionability) checks. Defaults to `false`.<a name="page-uncheck-option-force">#</a>
- `no_wait_after` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Actions that initiate navigations are waiting for these navigations to happen and for pages to start loading. You can opt out of waiting via setting this flag. You would only need this option in the exceptional cases such as navigating to inaccessible pages. Defaults to `false`.<a name="page-uncheck-option-no-wait-after">#</a>
- `position` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)> A point to use relative to the top-left corner of element padding box. If not specified, uses some visible point of the element.<a name="page-uncheck-option-position">#</a>
    - `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
    - `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-uncheck-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-uncheck-option-timeout">#</a>
- `trial` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When set, this method only performs the [actionability](https://playwright.dev/python/docs/actionability) checks and skips the action. Defaults to `false`. Useful to wait until the element is ready for the action without performing it.<a name="page-uncheck-option-trial">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-uncheck-return">#</a>

This method unchecks an element matching `selector` by performing the following steps:

1. Find an element matching `selector`. If there is none, wait until a matching element is attached to the DOM.
2. Ensure that matched element is a checkbox or a radio input. If not, this method throws. If the element is already unchecked, this method returns immediately.
3. Wait for [actionability](https://playwright.dev/python/docs/actionability) checks on the matched element, unless `force` option is set. If the element is detached during the checks, the whole action is retried.
4. Scroll the element into view if needed.
5. Use [page.mouse](#page-mouse) to click in the center of the element.
6. Wait for initiated navigations to either succeed or fail, unless `no_wait_after` option is set.
7. Ensure that the element is now unchecked. If not, this method throws.

When all steps combined have not finished during the specified `timeout`, this method throws a [TimeoutError](#timeouterror). Passing zero timeout disables this.

Shortcut for main frame's [frame.uncheck(selector, **kwargs)](#frame-uncheck).

## page.unroute(url, **kwargs)<a name="page-unroute">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[URL](https://en.wikipedia.org/wiki/URL)]:[bool](https://docs.python.org/3/library/stdtypes.html)> A glob pattern, regex pattern or predicate receiving [URL](https://en.wikipedia.org/wiki/URL) to match while routing.<a name="page-unroute-option-url">#</a>
- `handler` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[Route](#route), [Request](#request)]> Optional handler function to route the request.<a name="page-unroute-option-handler">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-unroute-return">#</a>

Removes a route created with [page.route(url, handler, **kwargs)](#page-route). When `handler` is not specified, removes all routes for the `url`.

## page.url<a name="page-url">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="page-url-return">#</a>

Shortcut for main frame's [frame.url](#frame-url).

## page.video<a name="page-video">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Video](#video)><a name="page-video-return">#</a>

Video object associated with this page.

## page.viewport_size<a name="page-viewport-size">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="page-viewport-size-return">#</a>
    - `width` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page width in pixels.
    - `height` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> page height in pixels.

## page.wait_for_event(event, **kwargs)<a name="page-wait-for-event-2">#</a>

- `event` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Event name, same one typically passed into `*.on(event)`.<a name="page-wait-for-event-2-option-event">#</a>
- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Receives the event data and resolves to truthy value when the waiting should resolve.<a name="page-wait-for-event-2-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-event-2-option-timeout">#</a>
- returns: \<[Any](https://docs.python.org/3/library/typing.html#typing.Any)><a name="page-wait-for-event-2-return">#</a>

> NOTE
>
> In most cases, you should use [page.expect_event(event, **kwargs)](#page-wait-for-event).

Waits for given `event` to fire. If predicate is provided, it passes event's value into the `predicate` function and waits for `predicate(event)` to return a truthy value. Will throw an error if the page is closed before the `event` is fired.

## page.wait_for_function(expression, **kwargs)<a name="page-wait-for-function">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="page-wait-for-function-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="page-wait-for-function-option-arg">#</a>
- `polling` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)|"raf"> If `polling` is `'raf'`, then `expression` is constantly executed in `requestAnimationFrame` callback. If `polling` is a number, then it is treated as an interval in milliseconds at which the function would be executed. Defaults to `raf`.<a name="page-wait-for-function-option-polling">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="page-wait-for-function-option-timeout">#</a>
- returns: \<[JSHandle](#jshandle)><a name="page-wait-for-function-return">#</a>

Returns when the `expression` returns a truthy value. It resolves to a JSHandle of the truthy value.

The [page.wait_for_function(expression, **kwargs)](#page-wait-for-function) can be used to observe viewport size change:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    webkit = playwright.webkit
    browser = webkit.launch()
    page = browser.new_page()
    page.evaluate("window.x = 0; setTimeout(() => { window.x = 100 }, 1000);")
    page.wait_for_function("() => window.x > 0")
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    webkit = playwright.webkit
    browser = await webkit.launch()
    page = await browser.new_page()
    await page.evaluate("window.x = 0; setTimeout(() => { window.x = 100 }, 1000);")
    await page.wait_for_function("() => window.x > 0")
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

To pass an argument to the predicate of [page.wait_for_function(expression, **kwargs)](#page-wait-for-function) function:

- Sync

```python
selector = ".foo"
page.wait_for_function("selector => !!document.querySelector(selector)", selector)
```

- Async

```python
selector = ".foo"
await page.wait_for_function("selector => !!document.querySelector(selector)", selector)
```

Shortcut for main frame's [frame.wait_for_function(expression, **kwargs)](#frame-wait-for-function).

## page.wait_for_load_state(**kwargs)<a name="page-wait-for-load-state">#</a>

- `state` \<"load"|"domcontentloaded"|"networkidle"> Optional load state to wait for, defaults to `load`. If the state has been already reached while loading current document, the method resolves immediately. Can be one of:<a name="page-wait-for-load-state-option-state">#</a>
    - `'load'` - wait for the `load` event to be fired.
    - `'domcontentloaded'` - wait for the `DOMContentLoaded` event to be fired.
    - `'networkidle'` - wait until there are no network connections for at least `500` ms.
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-wait-for-load-state-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-wait-for-load-state-return">#</a>

Returns when the required load state has been reached.

This resolves when the page reaches a required load state, `load` by default. The navigation must have been committed when this method is called. If current document has already reached the required state, resolves immediately.

- Sync

```python
page.click("button") # click triggers navigation.
page.wait_for_load_state() # the promise resolves after "load" event.
```

- Async

```python
await page.click("button") # click triggers navigation.
await page.wait_for_load_state() # the promise resolves after "load" event.
```

- Sync

```python
with page.expect_popup() as page_info:
    page.click("button") # click triggers a popup.
popup = page_info.value
 # Following resolves after "domcontentloaded" event.
popup.wait_for_load_state("domcontentloaded")
print(popup.title()) # popup is ready to use.
```

- Async

```python
async with page.expect_popup() as page_info:
    await page.click("button") # click triggers a popup.
popup = await page_info.value
 # Following resolves after "domcontentloaded" event.
await popup.wait_for_load_state("domcontentloaded")
print(await popup.title()) # popup is ready to use.
```

Shortcut for main frame's [frame.wait_for_load_state(**kwargs)](#frame-wait-for-load-state).

## page.wait_for_selector(selector, **kwargs)<a name="page-wait-for-selector">#</a>

- `selector` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> A selector to query for. See [working with selectors](https://playwright.dev/python/docs/selectors) for more details.<a name="page-wait-for-selector-option-selector">#</a>
- `state` \<"attached"|"detached"|"visible"|"hidden"> Defaults to `'visible'`. Can be either:<a name="page-wait-for-selector-option-state">#</a>
    - `'attached'` - wait for element to be present in DOM.
    - `'detached'` - wait for element to not be present in DOM.
    - `'visible'` - wait for element to have non-empty bounding box and no `visibility:hidden`. Note that element without any content or with `display:none` has an empty bounding box and is not considered visible.
    - `'hidden'` - wait for element to be either detached from DOM, or have an empty bounding box or `visibility:hidden`. This is opposite to the `'visible'` option.
- `strict` \<[bool](https://docs.python.org/3/library/stdtypes.html)> When true, the call requires selector to resolve to a single element. If given selector resolves to more then one element, the call throws an exception.<a name="page-wait-for-selector-option-strict">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-wait-for-selector-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[ElementHandle](#elementhandle)><a name="page-wait-for-selector-return">#</a>

Returns when element specified by selector satisfies `state` option. Returns `null` if waiting for `hidden` or `detached`.

> NOTE
>
> Playwright automatically waits for element to be ready before performing an action. Using [Locator](#locator) objects and web-first assertions make the code wait-for-selector-free.

Wait for the `selector` to satisfy `state` option (either appear/disappear from dom, or become visible/hidden). If at the moment of calling the method `selector` already satisfies the condition, the method will return immediately. If the selector doesn't satisfy the condition for the `timeout` milliseconds, the function will throw.

This method works across navigations:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    chromium = playwright.chromium
    browser = chromium.launch()
    page = browser.new_page()
    for current_url in ["https://google.com", "https://bbc.com"]:
        page.goto(current_url, wait_until="domcontentloaded")
        element = page.wait_for_selector("img")
        print("Loaded image: " + str(element.get_attribute("src")))
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    chromium = playwright.chromium
    browser = await chromium.launch()
    page = await browser.new_page()
    for current_url in ["https://google.com", "https://bbc.com"]:
        await page.goto(current_url, wait_until="domcontentloaded")
        element = await page.wait_for_selector("img")
        print("Loaded image: " + str(await element.get_attribute("src")))
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)
asyncio.run(main())
```

## page.wait_for_timeout(timeout)<a name="page-wait-for-timeout">#</a>

- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> A timeout to wait for<a name="page-wait-for-timeout-option-timeout">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-wait-for-timeout-return">#</a>

Waits for the given `timeout` in milliseconds.

Note that `page.waitForTimeout()` should only be used for debugging. Tests using the timer in production are going to be flaky. Use signals such as network events, selectors becoming visible and others instead.

- Sync

```python
# wait for 1 second
page.wait_for_timeout(1000)
```

- Async

```python
# wait for 1 second
await page.wait_for_timeout(1000)
```

Shortcut for main frame's [frame.wait_for_timeout(timeout)](#frame-wait-for-timeout).

## page.wait_for_url(url, **kwargs)<a name="page-wait-for-url">#</a>

- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[Pattern](https://docs.python.org/3/library/re.html)|[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)[[URL](https://en.wikipedia.org/wiki/URL)]:[bool](https://docs.python.org/3/library/stdtypes.html)> A glob pattern, regex pattern or predicate receiving [URL](https://en.wikipedia.org/wiki/URL) to match while waiting for the navigation. Note that if the parameter is a string without wilcard characters, the method will wait for navigation to URL that is exactly equal to the string.<a name="page-wait-for-url-option-url">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum operation time in milliseconds, defaults to 30 seconds, pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_navigation_timeout(timeout)](#browser-context-set-default-navigation-timeout), [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout), [page.set_default_navigation_timeout(timeout)](#page-set-default-navigation-timeout) or [page.set_default_timeout(timeout)](#page-set-default-timeout) methods.<a name="page-wait-for-url-option-timeout">#</a>
- `wait_until` \<"load"|"domcontentloaded"|"networkidle"|"commit"> When to consider operation succeeded, defaults to `load`. Events can be either:<a name="page-wait-for-url-option-wait-until">#</a>
    - `'domcontentloaded'` - consider operation to be finished when the `DOMContentLoaded` event is fired.
    - `'load'` - consider operation to be finished when the `load` event is fired.
    - `'networkidle'` - consider operation to be finished when there are no network connections for at least `500` ms.
    - `'commit'` - consider operation to be finished when network response is received and the document started loading.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="page-wait-for-url-return">#</a>

Waits for the main frame to navigate to the given URL.

- Sync

```python
page.click("a.delayed-navigation") # clicking the link will indirectly cause a navigation
page.wait_for_url("**/target.html")
```

- Async

```python
await page.click("a.delayed-navigation") # clicking the link will indirectly cause a navigation
await page.wait_for_url("**/target.html")
```

Shortcut for main frame's [frame.wait_for_url(url, **kwargs)](#frame-wait-for-url).

## page.workers<a name="page-workers">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Worker](#worker)]><a name="page-workers-return">#</a>

This method returns all of the dedicated [WebWorkers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API) associated with the page.

> NOTE
>
> This does not contain ServiceWorkers

## page.accessibility<a name="page-accessibility">#</a>

- type: \<[Accessibility](#accessibility)>

## page.keyboard<a name="page-keyboard">#</a>

- type: \<[Keyboard](#keyboard)>

## page.mouse<a name="page-mouse">#</a>

- type: \<[Mouse](#mouse)>

## page.request<a name="page-request">#</a>

- type: \<[APIRequestContext](#apirequestcontext)>

API testing helper associated with this page. Requests made with this API will use page cookies.

## page.touchscreen<a name="page-touchscreen">#</a>

- type: \<[Touchscreen](#touchscreen)>





# Request

Whenever the page sends a request for a network resource the following sequence of events are emitted by [Page](#page):

- [page.on("request")](#page-event-request) emitted when the request is issued by the page.
- [page.on("response")](#page-event-response) emitted when/if the response status and headers are received for the request.
- [page.on("requestfinished")](#page-event-request-finished) emitted when the response body is downloaded and the request is complete.

If request fails at some point, then instead of `'requestfinished'` event (and possibly instead of 'response' event), the [page.on("requestfailed")](#page-event-request-failed) event is emitted.

> NOTE
>
> HTTP Error responses, such as 404 or 503, are still successful responses from HTTP standpoint, so request will complete with `'requestfinished'` event.

If request gets a 'redirect' response, the request is successfully finished with the 'requestfinished' event, and a new request is issued to a redirected url.

- [request.all_headers()](#request-all-headers)
- [request.failure](#request-failure)
- [request.frame](#request-frame)
- [request.header_value(name)](#request-header-value)
- [request.headers](#request-headers)
- [request.headers_array()](#request-headers-array)
- [request.is_navigation_request()](#request-is-navigation-request)
- [request.method](#request-method)
- [request.post_data](#request-post-data)
- [request.post_data_buffer](#request-post-data-buffer)
- [request.post_data_json](#request-post-data-json)
- [request.redirected_from](#request-redirected-from)
- [request.redirected_to](#request-redirected-to)
- [request.resource_type](#request-resource-type)
- [request.response()](#request-response)
- [request.sizes()](#request-sizes)
- [request.timing](#request-timing)
- [request.url](#request-url)

## request.all_headers()<a name="request-all-headers">#</a>

- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="request-all-headers-return">#</a>

An object with all the request HTTP headers associated with this request. The header names are lower-cased.

## request.failure<a name="request-failure">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="request-failure-return">#</a>

The method returns `null` unless this request has failed, as reported by `requestfailed` event.

Example of logging of all the failed requests:

```python
page.on("requestfailed", lambda request: print(request.url + " " + request.failure))
```

## request.frame<a name="request-frame">#</a>

- returns: \<[Frame](#frame)><a name="request-frame-return">#</a>

Returns the [Frame](#frame) that initiated this request.

## request.header_value(name)<a name="request-header-value">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the header.<a name="request-header-value-option-name">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="request-header-value-return">#</a>

Returns the value of the header matching the name. The name is case insensitive.

## request.headers<a name="request-headers">#</a>

- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="request-headers-return">#</a>

**DEPRECATED** Incomplete list of headers as seen by the rendering engine. Use [request.all_headers()](#request-all-headers) instead.

## request.headers_array()<a name="request-headers-array">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="request-headers-array-return">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the header.
    - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Value of the header.

An array with all the request HTTP headers associated with this request. Unlike [request.all_headers()](#request-all-headers), header names are NOT lower-cased. Headers with multiple entries, such as `Set-Cookie`, appear in the array multiple times.

## request.is_navigation_request()<a name="request-is-navigation-request">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="request-is-navigation-request-return">#</a>

Whether this request is driving frame's navigation.

## request.method<a name="request-method">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="request-method-return">#</a>

Request's method (GET, POST, etc.)

## request.post_data<a name="request-post-data">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="request-post-data-return">#</a>

Request's post body, if any.

## request.post_data_buffer<a name="request-post-data-buffer">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)><a name="request-post-data-buffer-return">#</a>

Request's post body in a binary form, if any.

## request.post_data_json<a name="request-post-data-json">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="request-post-data-json-return">#</a>

Returns parsed request's body for `form-urlencoded` and JSON as a fallback if any.

When the response is `application/x-www-form-urlencoded` then a key/value object of the values will be returned. Otherwise it will be parsed as JSON.

## request.redirected_from<a name="request-redirected-from">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Request](#request)><a name="request-redirected-from-return">#</a>

Request that was redirected by the server to this one, if any.

When the server responds with a redirect, Playwright creates a new [Request](#request) object. The two requests are connected by `redirectedFrom()` and `redirectedTo()` methods. When multiple server redirects has happened, it is possible to construct the whole redirect chain by repeatedly calling `redirectedFrom()`.

For example, if the website `http://example.com` redirects to `https://example.com`:

- Sync

```python
response = page.goto("http://example.com")
print(response.request.redirected_from.url) # "http://example.com"
```

- Async

```python
response = await page.goto("http://example.com")
print(response.request.redirected_from.url) # "http://example.com"
```

If the website `https://google.com` has no redirects:

- Sync

```python
response = page.goto("https://google.com")
print(response.request.redirected_from) # None
```

- Async

```python
response = await page.goto("https://google.com")
print(response.request.redirected_from) # None
```

## request.redirected_to<a name="request-redirected-to">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Request](#request)><a name="request-redirected-to-return">#</a>

New request issued by the browser if the server responded with redirect.

This method is the opposite of [request.redirected_from](#request-redirected-from):

```python
assert request.redirected_from.redirected_to == request
```

## request.resource_type<a name="request-resource-type">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="request-resource-type-return">#</a>

Contains the request's resource type as it was perceived by the rendering engine. ResourceType will be one of the following: `document`, `stylesheet`, `image`, `media`, `font`, `script`, `texttrack`, `xhr`, `fetch`, `eventsource`, `websocket`, `manifest`, `other`.

## request.response()<a name="request-response">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Response](#response)><a name="request-response-return">#</a>

Returns the matching [Response](#response) object, or `null` if the response was not received due to error.

## request.sizes()<a name="request-sizes">#</a>

- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="request-sizes-return">#</a>
    - `requestBodySize` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Size of the request body (POST data payload) in bytes. Set to 0 if there was no body.
    - `requestHeadersSize` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Total number of bytes from the start of the HTTP request message until (and including) the double CRLF before the body.
    - `responseBodySize` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Size of the received response body (encoded) in bytes.
    - `responseHeadersSize` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Total number of bytes from the start of the HTTP response message until (and including) the double CRLF before the body.

Returns resource size information for given request.

## request.timing<a name="request-timing">#</a>

- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="request-timing-return">#</a>
    - `startTime` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Request start time in milliseconds elapsed since January 1, 1970 00:00:00 UTC
    - `domainLookupStart` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time immediately before the browser starts the domain name lookup for the resource. The value is given in milliseconds relative to `startTime`, -1 if not available.
    - `domainLookupEnd` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time immediately after the browser starts the domain name lookup for the resource. The value is given in milliseconds relative to `startTime`, -1 if not available.
    - `connectStart` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time immediately before the user agent starts establishing the connection to the server to retrieve the resource. The value is given in milliseconds relative to `startTime`, -1 if not available.
    - `secureConnectionStart` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time immediately before the browser starts the handshake process to secure the current connection. The value is given in milliseconds relative to `startTime`, -1 if not available.
    - `connectEnd` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time immediately before the user agent starts establishing the connection to the server to retrieve the resource. The value is given in milliseconds relative to `startTime`, -1 if not available.
    - `requestStart` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time immediately before the browser starts requesting the resource from the server, cache, or local resource. The value is given in milliseconds relative to `startTime`, -1 if not available.
    - `responseStart` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time immediately after the browser starts requesting the resource from the server, cache, or local resource. The value is given in milliseconds relative to `startTime`, -1 if not available.
    - `responseEnd` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Time immediately after the browser receives the last byte of the resource or immediately before the transport connection is closed, whichever comes first. The value is given in milliseconds relative to `startTime`, -1 if not available.

Returns resource timing information for given request. Most of the timing values become available upon the response, `responseEnd` becomes available when request finishes. Find more information at [Resource Timing API](https://developer.mozilla.org/en-US/docs/Web/API/PerformanceResourceTiming).

- Sync

```python
with page.expect_event("requestfinished") as request_info:
    page.goto("http://example.com")
request = request_info.value
print(request.timing)
```

- Async

```python
async with page.expect_event("requestfinished") as request_info:
    await page.goto("http://example.com")
request = await request_info.value
print(request.timing)
```

## request.url<a name="request-url">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="request-url-return">#</a>

URL of the request.



# Response

[Response](#response) class represents responses which are received by page.

- [response.all_headers()](#response-all-headers)
- [response.body()](#response-body)
- [response.finished()](#response-finished)
- [response.frame](#response-frame)
- [response.header_value(name)](#response-header-value)
- [response.header_values(name)](#response-header-values)
- [response.headers](#response-headers)
- [response.headers_array()](#response-headers-array)
- [response.json()](#response-json)
- [response.ok](#response-ok)
- [response.request](#response-request)
- [response.security_details()](#response-security-details)
- [response.server_addr()](#response-server-addr)
- [response.status](#response-status)
- [response.status_text](#response-status-text)
- [response.text()](#response-text)
- [response.url](#response-url)

## response.all_headers()<a name="response-all-headers">#</a>

- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="response-all-headers-return">#</a>

An object with all the response HTTP headers associated with this response.

## response.body()<a name="response-body">#</a>

- returns: \<[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)><a name="response-body-return">#</a>

Returns the buffer with response body.

## response.finished()<a name="response-finished">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="response-finished-return">#</a>

Waits for this response to finish, returns always `null`.

## response.frame<a name="response-frame">#</a>

- returns: \<[Frame](#frame)><a name="response-frame-return">#</a>

Returns the [Frame](#frame) that initiated this response.

## response.header_value(name)<a name="response-header-value">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the header.<a name="response-header-value-option-name">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="response-header-value-return">#</a>

Returns the value of the header matching the name. The name is case insensitive. If multiple headers have the same name (except `set-cookie`), they are returned as a list separated by `, `. For `set-cookie`, the `\n` separator is used. If no headers are found, `null` is returned.

## response.header_values(name)<a name="response-header-values">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the header.<a name="response-header-values-option-name">#</a>
- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="response-header-values-return">#</a>

Returns all values of the headers matching the name, for example `set-cookie`. The name is case insensitive.

## response.headers<a name="response-headers">#</a>

- returns: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]><a name="response-headers-return">#</a>

**DEPRECATED** Incomplete list of headers as seen by the rendering engine. Use [response.all_headers()](#response-all-headers) instead.

## response.headers_array()<a name="response-headers-array">#</a>

- returns: \<[List](https://docs.python.org/3/library/typing.html#typing.List)[[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)]><a name="response-headers-array-return">#</a>
    - `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name of the header.
    - `value` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Value of the header.

An array with all the request HTTP headers associated with this response. Unlike [response.all_headers()](#response-all-headers), header names are NOT lower-cased. Headers with multiple entries, such as `Set-Cookie`, appear in the array multiple times.

## response.json()<a name="response-json">#</a>

- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="response-json-return">#</a>

Returns the JSON representation of response body.

This method will throw if the response body is not parsable via `JSON.parse`.

## response.ok<a name="response-ok">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="response-ok-return">#</a>

Contains a boolean stating whether the response was successful (status in the range 200-299) or not.

## response.request<a name="response-request">#</a>

- returns: \<[Request](#request)><a name="response-request-return">#</a>

Returns the matching [Request](#request) object.

## response.security_details()<a name="response-security-details">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="response-security-details-return">#</a>
    - `issuer` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Common Name component of the Issuer field. from the certificate. This should only be used for informational purposes. Optional.
    - `protocol` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> The specific TLS protocol used. (e.g. `TLS 1.3`). Optional.
    - `subjectName` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Common Name component of the Subject field from the certificate. This should only be used for informational purposes. Optional.
    - `validFrom` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Unix timestamp (in seconds) specifying when this cert becomes valid. Optional.
    - `validTo` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Unix timestamp (in seconds) specifying when this cert becomes invalid. Optional.

Returns SSL and other security information.

## response.server_addr()<a name="response-server-addr">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)|[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)><a name="response-server-addr-return">#</a>
    - `ipAddress` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> IPv4 or IPV6 address of the server.
    - `port` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)>

Returns the IP address and port of the server.

## response.status<a name="response-status">#</a>

- returns: \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="response-status-return">#</a>

Contains the status code of the response (e.g., 200 for a success).

## response.status_text<a name="response-status-text">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="response-status-text-return">#</a>

Contains the status text of the response (e.g. usually an "OK" for a success).

## response.text()<a name="response-text">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="response-text-return">#</a>

Returns the text representation of response body.

## response.url<a name="response-url">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="response-url-return">#</a>

Contains the URL of the response.



# Route

Whenever a network route is set up with [page.route(url, handler, **kwargs)](#page-route) or [browser_context.route(url, handler, **kwargs)](#browser-context-route), the `Route` object allows to handle the route.

- [route.abort(**kwargs)](#route-abort)
- [route.continue_(**kwargs)](#route-continue)
- [route.fulfill(**kwargs)](#route-fulfill)
- [route.request](#route-request)

## route.abort(**kwargs)<a name="route-abort">#</a>

- `error_code` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Optional error code. Defaults to `failed`, could be one of the following:<a name="route-abort-option-error-code">#</a>
    - `'aborted'` - An operation was aborted (due to user action)
    - `'accessdenied'` - Permission to access a resource, other than the network, was denied
    - `'addressunreachable'` - The IP address is unreachable. This usually means that there is no route to the specified host or network.
    - `'blockedbyclient'` - The client chose to block the request.
    - `'blockedbyresponse'` - The request failed because the response was delivered along with requirements which are not met ('X-Frame-Options' and 'Content-Security-Policy' ancestor checks, for instance).
    - `'connectionaborted'` - A connection timed out as a result of not receiving an ACK for data sent.
    - `'connectionclosed'` - A connection was closed (corresponding to a TCP FIN).
    - `'connectionfailed'` - A connection attempt failed.
    - `'connectionrefused'` - A connection attempt was refused.
    - `'connectionreset'` - A connection was reset (corresponding to a TCP RST).
    - `'internetdisconnected'` - The Internet connection has been lost.
    - `'namenotresolved'` - The host name could not be resolved.
    - `'timedout'` - An operation timed out.
    - `'failed'` - A generic failure occurred.
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="route-abort-return">#</a>

Aborts the route's request.

## route.continue_(**kwargs)<a name="route-continue">#</a>

- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> If set changes the request HTTP headers. Header values will be converted to a string.<a name="route-continue-option-headers">#</a>
- `method` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> If set changes the request method (e.g. GET or POST)<a name="route-continue-option-method">#</a>
- `post_data` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> If set changes the post data of request<a name="route-continue-option-post-data">#</a>
- `url` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> If set changes the request URL. New URL must have same protocol as original one.<a name="route-continue-option-url">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="route-continue-return">#</a>

Continues route's request with optional overrides.

- Sync

```python
def handle(route, request):
    # override headers
    headers = {
        **request.headers,
        "foo": "bar" # set "foo" header
        "origin": None # remove "origin" header
    }
    route.continue_(headers=headers)
}
page.route("**/*", handle)
```

- Async

```python
async def handle(route, request):
    # override headers
    headers = {
        **request.headers,
        "foo": "bar" # set "foo" header
        "origin": None # remove "origin" header
    }
    await route.continue_(headers=headers)
}
await page.route("**/*", handle)
```

## route.fulfill(**kwargs)<a name="route-fulfill">#</a>

- `body` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> Response body.<a name="route-fulfill-option-body">#</a>
- `content_type` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> If set, equals to setting `Content-Type` response header.<a name="route-fulfill-option-content-type">#</a>
- `headers` \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)]> Response headers. Header values will be converted to a string.<a name="route-fulfill-option-headers">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> File path to respond with. The content type will be inferred from file extension. If `path` is a relative path, then it is resolved relative to the current working directory.<a name="route-fulfill-option-path">#</a>
- `response` \<[APIResponse](#apiresponse)> [APIResponse](#apiresponse) to fulfill route's request with. Individual fields of the response (such as headers) can be overridden using fulfill options.<a name="route-fulfill-option-response">#</a>
- `status` \<[int](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Response status code, defaults to `200`.<a name="route-fulfill-option-status">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="route-fulfill-return">#</a>

Fulfills route's request with given response.

An example of fulfilling all requests with 404 responses:

- Sync

```python
page.route("**/*", lambda route: route.fulfill(
    status=404,
    content_type="text/plain",
    body="not found!"))
```

- Async

```python
await page.route("**/*", lambda route: route.fulfill(
    status=404,
    content_type="text/plain",
    body="not found!"))
```

An example of serving static file:

- Sync

```python
page.route("**/xhr_endpoint", lambda route: route.fulfill(path="mock_data.json"))
```

- Async

```python
await page.route("**/xhr_endpoint", lambda route: route.fulfill(path="mock_data.json"))
```

## route.request<a name="route-request">#</a>

- returns: \<[Request](#request)><a name="route-request-return">#</a>

A request to be routed.



# Selectors

Selectors can be used to install custom selector engines. See [Working with selectors](https://playwright.dev/python/docs/selectors) for more information.

- [selectors.register(name, **kwargs)](#selectors-register)

## selectors.register(name, **kwargs)<a name="selectors-register">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Name that is used in selectors as a prefix, e.g. `{name: 'foo'}` enables `foo=myselectorbody` selectors. May only contain `[a-zA-Z0-9_]` characters.<a name="selectors-register-option-name">#</a>
- `content_script` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to run this selector engine in isolated JavaScript environment. This environment has access to the same DOM, but not any JavaScript objects from the frame's scripts. Defaults to `false`. Note that running as a content script is not guaranteed when this engine is used together with other registered engines.<a name="selectors-register-option-content-script">#</a>
- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path to the JavaScript file. If `path` is a relative path, then it is resolved relative to the current working directory.<a name="selectors-register-option-path">#</a>
- `script` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Raw script content.<a name="selectors-register-option-script">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="selectors-register-return">#</a>

An example of registering selector engine that queries elements based on a tag name:

- Sync

```python
from playwright.sync_api import sync_playwright

def run(playwright):
    tag_selector = """
      {
          // Returns the first element matching given selector in the root's subtree.
          query(root, selector) {
              return root.querySelector(selector);
          },
          // Returns all elements matching given selector in the root's subtree.
          queryAll(root, selector) {
              return Array.from(root.querySelectorAll(selector));
          }
      }"""

    # Register the engine. Selectors will be prefixed with "tag=".
    playwright.selectors.register("tag", tag_selector)
    browser = playwright.chromium.launch()
    page = browser.new_page()
    page.set_content('\<div>\<button>Click me\</button>\</div>')

    # Use the selector prefixed with its name.
    button = page.locator('tag=button')
    # Combine it with other selector engines.
    page.click('tag=div >> text="Click me"')
    # Can use it in any methods supporting selectors.
    button_count = page.locator('tag=button').count()
    print(button_count)
    browser.close()

with sync_playwright() as playwright:
    run(playwright)
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright

async def run(playwright):
    tag_selector = """
      {
          // Returns the first element matching given selector in the root's subtree.
          query(root, selector) {
              return root.querySelector(selector);
          },
          // Returns all elements matching given selector in the root's subtree.
          queryAll(root, selector) {
              return Array.from(root.querySelectorAll(selector));
          }
      }"""

    # Register the engine. Selectors will be prefixed with "tag=".
    await playwright.selectors.register("tag", tag_selector)
    browser = await playwright.chromium.launch()
    page = await browser.new_page()
    await page.set_content('\<div>\<button>Click me\</button>\</div>')

    # Use the selector prefixed with its name.
    button = await page.query_selector('tag=button')
    # Combine it with other selector engines.
    await page.click('tag=div >> text="Click me"')
    # Can use it in any methods supporting selectors.
    button_count = await page.locator('tag=button').count()
    print(button_count)
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)

asyncio.run(main())
```



# TimeoutError

- extends: [Error](#error)

TimeoutError is emitted whenever certain operations are terminated due to timeout, e.g. [locator.wait_for(**kwargs)](#locator-wait-for) or [browser_type.launch(**kwargs)](#browser-type-launch).

- Sync

```python
from playwright.sync_api import sync_playwright, TimeoutError as PlaywrightTimeoutError

with sync_playwright() as p:
    browser = p.chromium.launch()
    page = browser.new_page()
    try:
      page.click("text=Example", timeout=100)
    except PlaywrightTimeoutError:
      print("Timeout!")
    browser.close()
```

- Async

```python
import asyncio
from playwright.async_api import async_playwright, TimeoutError as PlaywrightTimeoutError

async def run(playwright):
    browser = await playwright.chromium.launch()
    page = await browser.new_page()
    try:
      await page.click("text=Example", timeout=100)
    except PlaywrightTimeoutError:
      print("Timeout!")
    await browser.close()

async def main():
    async with async_playwright() as playwright:
        await run(playwright)

asyncio.run(main())
```



# Touchscreen

The Touchscreen class operates in main-frame CSS pixels relative to the top-left corner of the viewport. Methods on the touchscreen can only be used in browser contexts that have been initialized with `hasTouch` set to true.

- [touchscreen.tap(x, y)](#touchscreen-tap)

## touchscreen.tap(x, y)<a name="touchscreen-tap">#</a>

- `x` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="touchscreen-tap-option-x">#</a>
- `y` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)><a name="touchscreen-tap-option-y">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="touchscreen-tap-return">#</a>

Dispatches a `touchstart` and `touchend` event with a single touch at the position (`x`,`y`).



# Tracing

API for collecting and saving Playwright traces. Playwright traces can be opened in [Trace Viewer](https://playwright.dev/python/docs/trace-viewer) after Playwright script runs.

Start recording a trace before performing actions. At the end, stop tracing and save it to a file.

- Sync

```python
browser = chromium.launch()
context = browser.new_context()
context.tracing.start(screenshots=True, snapshots=True)
page = context.new_page()
page.goto("https://playwright.dev")
context.tracing.stop(path = "trace.zip")
```

- Async

```python
browser = await chromium.launch()
context = await browser.new_context()
await context.tracing.start(screenshots=True, snapshots=True)
page = await context.new_page()
await page.goto("https://playwright.dev")
await context.tracing.stop(path = "trace.zip")
```

- [tracing.start(**kwargs)](#tracing-start)
- [tracing.start_chunk(**kwargs)](#tracing-start-chunk)
- [tracing.stop(**kwargs)](#tracing-stop)
- [tracing.stop_chunk(**kwargs)](#tracing-stop-chunk)

## tracing.start(**kwargs)<a name="tracing-start">#</a>

- `name` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> If specified, the trace is going to be saved into the file with the given name inside the `traces_dir` folder specified in [browser_type.launch(**kwargs)](#browser-type-launch).<a name="tracing-start-option-name">#</a>
- `screenshots` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to capture screenshots during tracing. Screenshots are used to build a timeline preview.<a name="tracing-start-option-screenshots">#</a>
- `snapshots` \<[bool](https://docs.python.org/3/library/stdtypes.html)> If this option is true tracing will<a name="tracing-start-option-snapshots">#</a>
    - capture DOM snapshot on every action
    - record network activity
- `sources` \<[bool](https://docs.python.org/3/library/stdtypes.html)> Whether to include source files for trace actions.<a name="tracing-start-option-sources">#</a>
- `title` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Trace name to be shown in the Trace Viewer.<a name="tracing-start-option-title">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="tracing-start-return">#</a>

Start tracing.

- Sync

```python
context.tracing.start(name="trace", screenshots=True, snapshots=True)
page = context.new_page()
page.goto("https://playwright.dev")
context.tracing.stop(path = "trace.zip")
```

- Async

```python
await context.tracing.start(name="trace", screenshots=True, snapshots=True)
page = await context.new_page()
await page.goto("https://playwright.dev")
await context.tracing.stop(path = "trace.zip")
```

## tracing.start_chunk(**kwargs)<a name="tracing-start-chunk">#</a>

- `title` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Trace name to be shown in the Trace Viewer.<a name="tracing-start-chunk-option-title">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="tracing-start-chunk-return">#</a>

Start a new trace chunk. If you'd like to record multiple traces on the same [BrowserContext](#browsercontext), use [tracing.start(**kwargs)](#tracing-start) once, and then create multiple trace chunks with [tracing.start_chunk(**kwargs)](#tracing-start-chunk) and [tracing.stop_chunk(**kwargs)](#tracing-stop-chunk).

- Sync

```python
context.tracing.start(name="trace", screenshots=True, snapshots=True)
page = context.new_page()
page.goto("https://playwright.dev")

context.tracing.start_chunk()
page.click("text=Get Started")
# Everything between start_chunk and stop_chunk will be recorded in the trace.
context.tracing.stop_chunk(path = "trace1.zip")

context.tracing.start_chunk()
page.goto("http://example.com")
# Save a second trace file with different actions.
context.tracing.stop_chunk(path = "trace2.zip")
```

- Async

```python
await context.tracing.start(name="trace", screenshots=True, snapshots=True)
page = await context.new_page()
await page.goto("https://playwright.dev")

await context.tracing.start_chunk()
await page.click("text=Get Started")
# Everything between start_chunk and stop_chunk will be recorded in the trace.
await context.tracing.stop_chunk(path = "trace1.zip")

await context.tracing.start_chunk()
await page.goto("http://example.com")
# Save a second trace file with different actions.
await context.tracing.stop_chunk(path = "trace2.zip")
```

## tracing.stop(**kwargs)<a name="tracing-stop">#</a>

- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Export trace into the file with the given path.<a name="tracing-stop-option-path">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="tracing-stop-return">#</a>

Stop tracing.

## tracing.stop_chunk(**kwargs)<a name="tracing-stop-chunk">#</a>

- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Export trace collected since the last [tracing.start_chunk(**kwargs)](#tracing-start-chunk) call into the file with the given path.<a name="tracing-stop-chunk-option-path">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="tracing-stop-chunk-return">#</a>

Stop the trace chunk. See [tracing.start_chunk(**kwargs)](#tracing-start-chunk) for more details about multiple trace chunks.



# Video

When browser context is created with the `recordVideo` option, each page has a video object associated with it.

- Sync

```python
print(page.video.path())
```

- Async

```python
print(await page.video.path())
```

- [video.delete()](#video-delete)
- [video.path()](#video-path)
- [video.save_as(path)](#video-save-as)

## video.delete()<a name="video-delete">#</a>

- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="video-delete-return">#</a>

Deletes the video file. Will wait for the video to finish if necessary.

## video.path()<a name="video-path">#</a>

- returns: \<[pathlib.Path](https://realpython.com/python-pathlib/)><a name="video-path-return">#</a>

Returns the file system path this video will be recorded to. The video is guaranteed to be written to the filesystem upon closing the browser context. This method throws when connected remotely.

## video.save_as(path)<a name="video-save-as">#</a>

- `path` \<[Union](https://docs.python.org/3/library/typing.html#typing.Union)[[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str), [pathlib.Path](https://realpython.com/python-pathlib/)]> Path where the video should be saved.<a name="video-save-as-option-path">#</a>
- returns: \<[NoneType](https://docs.python.org/3/library/constants.html#None)><a name="video-save-as-return">#</a>

Saves the video to a user-specified path. It is safe to call this method while the video is still in progress, or after the page has closed. This method waits until the page is closed and the video is fully saved.



# WebSocket

The [WebSocket](#websocket) class represents websocket connections in the page.

- [web_socket.on("close")](#web-socket-event-close)
- [web_socket.on("framereceived")](#web-socket-event-frame-received)
- [web_socket.on("framesent")](#web-socket-event-frame-sent)
- [web_socket.on("socketerror")](#web-socket-event-socket-error)
- [web_socket.expect_event(event, **kwargs)](#web-socket-wait-for-event)
- [web_socket.is_closed()](#web-socket-is-closed)
- [web_socket.url](#web-socket-url)
- [web_socket.wait_for_event(event, **kwargs)](#web-socket-wait-for-event-2)

## web_socket.on("close")<a name="web-socket-event-close">#</a>

- type: \<[WebSocket](#websocket)>

Fired when the websocket closes.

## web_socket.on("framereceived")<a name="web-socket-event-frame-received">#</a>

- type: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)>
    - `payload` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> frame payload

Fired when the websocket receives a frame.

## web_socket.on("framesent")<a name="web-socket-event-frame-sent">#</a>

- type: \<[Dict](https://docs.python.org/3/library/typing.html#typing.Dict)>
    - `payload` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)|[bytes](https://docs.python.org/3/library/stdtypes.html#bytes)> frame payload

Fired when the websocket sends a frame.

## web_socket.on("socketerror")<a name="web-socket-event-socket-error">#</a>

- type: \<[String]>

Fired when the websocket has an error.

## web_socket.expect_event(event, **kwargs)<a name="web-socket-wait-for-event">#</a>

- `event` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Event name, same one would pass into `webSocket.on(event)`.<a name="web-socket-wait-for-event-option-event">#</a>
- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Receives the event data and resolves to truthy value when the waiting should resolve.<a name="web-socket-wait-for-event-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="web-socket-wait-for-event-option-timeout">#</a>
- returns: \<[EventContextManager](https://docs.python.org/3/reference/datamodel.html#context-managers)><a name="web-socket-wait-for-event-return">#</a>

Waits for event to fire and passes its value into the predicate function. Returns when the predicate returns truthy value. Will throw an error if the webSocket is closed before the event is fired. Returns the event data value.

## web_socket.is_closed()<a name="web-socket-is-closed">#</a>

- returns: \<[bool](https://docs.python.org/3/library/stdtypes.html)><a name="web-socket-is-closed-return">#</a>

Indicates that the web socket has been closed.

## web_socket.url<a name="web-socket-url">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="web-socket-url-return">#</a>

Contains the URL of the WebSocket.

## web_socket.wait_for_event(event, **kwargs)<a name="web-socket-wait-for-event-2">#</a>

- `event` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> Event name, same one typically passed into `*.on(event)`.<a name="web-socket-wait-for-event-2-option-event">#</a>
- `predicate` \<[Callable](https://docs.python.org/3/library/typing.html#typing.Callable)> Receives the event data and resolves to truthy value when the waiting should resolve.<a name="web-socket-wait-for-event-2-option-predicate">#</a>
- `timeout` \<[float](https://docs.python.org/3/library/stdtypes.html#numeric-types-int-float-complex)> Maximum time to wait for in milliseconds. Defaults to `30000` (30 seconds). Pass `0` to disable timeout. The default value can be changed by using the [browser_context.set_default_timeout(timeout)](#browser-context-set-default-timeout).<a name="web-socket-wait-for-event-2-option-timeout">#</a>
- returns: \<[Any](https://docs.python.org/3/library/typing.html#typing.Any)><a name="web-socket-wait-for-event-2-return">#</a>

> NOTE
>
> In most cases, you should use [web_socket.expect_event(event, **kwargs)](#web-socket-wait-for-event).

Waits for given `event` to fire. If predicate is provided, it passes event's value into the `predicate` function and waits for `predicate(event)` to return a truthy value. Will throw an error if the socket is closed before the `event` is fired.



# Worker

The Worker class represents a [WebWorker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API). `worker` event is emitted on the page object to signal a worker creation. `close` event is emitted on the worker object when the worker is gone.

```python
def handle_worker(worker):
    print("worker created: " + worker.url)
    worker.on("close", lambda: print("worker destroyed: " + worker.url))

page.on('worker', handle_worker)

print("current workers:")
for worker in page.workers:
    print("    " + worker.url)
```

- [worker.on("close")](#worker-event-close)
- [worker.evaluate(expression, **kwargs)](#worker-evaluate)
- [worker.evaluate_handle(expression, **kwargs)](#worker-evaluate-handle)
- [worker.url](#worker-url)

## worker.on("close")<a name="worker-event-close">#</a>

- type: \<[Worker](#worker)>

Emitted when this dedicated [WebWorker](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API) is terminated.

## worker.evaluate(expression, **kwargs)<a name="worker-evaluate">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="worker-evaluate-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="worker-evaluate-option-arg">#</a>
- returns: \<[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description)><a name="worker-evaluate-return">#</a>

Returns the return value of `expression`.

If the function passed to the [worker.evaluate(expression, **kwargs)](#worker-evaluate) returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [worker.evaluate(expression, **kwargs)](#worker-evaluate) would wait for the promise to resolve and return its value.

If the function passed to the [worker.evaluate(expression, **kwargs)](#worker-evaluate) returns a non-[Serializable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify#Description) value, then [worker.evaluate(expression, **kwargs)](#worker-evaluate) returns `undefined`. Playwright also supports transferring some additional values that are not serializable by `JSON`: `-0`, `NaN`, `Infinity`, `-Infinity`.

## worker.evaluate_handle(expression, **kwargs)<a name="worker-evaluate-handle">#</a>

- `expression` \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)> JavaScript expression to be evaluated in the browser context. If it looks like a function declaration, it is interpreted as a function. Otherwise, evaluated as an expression.<a name="worker-evaluate-handle-option-expression">#</a>
- `arg` \<[EvaluationArgument](https://playwright.dev/python/docs/evaluating#evaluation-argument)> Optional argument to pass to `expression`.<a name="worker-evaluate-handle-option-arg">#</a>
- returns: \<[JSHandle](#jshandle)><a name="worker-evaluate-handle-return">#</a>

Returns the return value of `expression` as a [JSHandle](#jshandle).

The only difference between [worker.evaluate(expression, **kwargs)](#worker-evaluate) and [worker.evaluate_handle(expression, **kwargs)](#worker-evaluate-handle) is that [worker.evaluate_handle(expression, **kwargs)](#worker-evaluate-handle) returns [JSHandle](#jshandle).

If the function passed to the [worker.evaluate_handle(expression, **kwargs)](#worker-evaluate-handle) returns a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise), then [worker.evaluate_handle(expression, **kwargs)](#worker-evaluate-handle) would wait for the promise to resolve and return its value.

## worker.url<a name="worker-url">#</a>

- returns: \<[str](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)><a name="worker-url-return">#</a>

