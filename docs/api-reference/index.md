<a id="scrapling"></a>

# scrapling

<a id="scrapling.__dir__"></a>

#### \_\_dir\_\_

```python
def __dir__() -> list[str]
```

Support for dir() and autocomplete.

<a id="scrapling.spiders.session"></a>

# scrapling.spiders.session

<a id="scrapling.spiders.session.SessionManager"></a>

## SessionManager Objects

```python
class SessionManager()
```

Manages pre-configured session instances.

<a id="scrapling.spiders.session.SessionManager.add"></a>

#### add

```python
def add(session_id: str,
        session: Session,
        *,
        default: bool = False,
        lazy: bool = False) -> "SessionManager"
```

Register a session instance.

**Arguments**:

- `session_id`: Name to reference this session in requests
- `session`: Your pre-configured session instance
- `default`: If True, this becomes the default session
- `lazy`: If True, the session will be started only when a request uses its ID.

<a id="scrapling.spiders.session.SessionManager.remove"></a>

#### remove

```python
def remove(session_id: str) -> None
```

Removes a session.

**Arguments**:

- `session_id`: ID of session to remove

<a id="scrapling.spiders.session.SessionManager.pop"></a>

#### pop

```python
def pop(session_id: str) -> Session
```

Remove and returns a session.

**Arguments**:

- `session_id`: ID of session to remove

<a id="scrapling.spiders.session.SessionManager.start"></a>

#### start

```python
async def start() -> None
```

Start all sessions that aren't already alive.

<a id="scrapling.spiders.session.SessionManager.close"></a>

#### close

```python
async def close() -> None
```

Close all registered sessions.

<a id="scrapling.spiders.session.SessionManager.__contains__"></a>

#### \_\_contains\_\_

```python
def __contains__(session_id: str) -> bool
```

Check if a session ID is registered.

<a id="scrapling.spiders.session.SessionManager.__len__"></a>

#### \_\_len\_\_

```python
def __len__() -> int
```

Number of registered sessions.

<a id="scrapling.spiders.robotstxt"></a>

# scrapling.spiders.robotstxt

<a id="scrapling.spiders.robotstxt.RobotsTxtManager"></a>

## RobotsTxtManager Objects

```python
class RobotsTxtManager()
```

Manages fetching, parsing, and caching of robots.txt files.

<a id="scrapling.spiders.robotstxt.RobotsTxtManager.can_fetch"></a>

#### can\_fetch

```python
async def can_fetch(url: str, sid: str) -> bool
```

Check if a URL can be fetched according to the domain's robots.txt.

**Arguments**:

- `url`: The full URL to check
- `sid`: Session ID for fetching robots.txt if not yet cached

<a id="scrapling.spiders.robotstxt.RobotsTxtManager.get_delay_directives"></a>

#### get\_delay\_directives

```python
async def get_delay_directives(
        url: str,
        sid: str) -> tuple[Optional[float], Optional[tuple[int, int]]]
```

Return both crawl-delay and request-rate in a single parser lookup.

**Arguments**:

- `url`: Any URL on the domain to check
- `sid`: Session ID for fetching robots.txt if not yet cached

<a id="scrapling.spiders.robotstxt.RobotsTxtManager.prefetch"></a>

#### prefetch

```python
async def prefetch(urls: list[str], sid: str) -> None
```

Pre-warm the robots.txt cache for a list of seed URLs concurrently.

**Arguments**:

- `urls`: Seed URLs whose domains should be pre-fetched (one per domain).
- `sid`: Session ID to use for the robots.txt fetch requests.

<a id="scrapling.spiders.templates.crawler"></a>

# scrapling.spiders.templates.crawler

Generic spider templates that build on the `Spider` base.

<a id="scrapling.spiders.templates.crawler.CrawlRule"></a>

## CrawlRule Objects

```python
@dataclass
class CrawlRule()
```

Rule for `CrawlSpider`: extract links from a response and dispatch them.

**Arguments**:

- `link_extractor`: `LinkExtractor` that produces URLs from each response.
- `callback`: Bound method on the spider to call for each matched URL.
Falls back to the spider's default ``parse()`` by default.
- `priority`: Override the priority of the requests that will be dispatched.
- `process_request`: Optional bound method to mutate each `Request` before
it is yielded. Signature: ``(request, response) -> request``. Use it to
add headers, change priority, or filter requests.

<a id="scrapling.spiders.templates.crawler.CrawlSpider"></a>

## CrawlSpider Objects

```python
class CrawlSpider(Spider)
```

A generic spider that can extract and follow links automatically based on crawl rules.

Override `rules()` to return a list of `CrawlRule`s.

You can start from it and override it as needed for more custom functionality, or just implement your own spider.

<a id="scrapling.spiders.templates.crawler.CrawlSpider.rules"></a>

#### rules

```python
def rules() -> List[CrawlRule]
```

Override to define link-following rules.

<a id="scrapling.spiders.templates.sitemap"></a>

# scrapling.spiders.templates.sitemap

Sitemap template spider.

<a id="scrapling.spiders.templates.sitemap.SitemapResult"></a>

## SitemapResult Objects

```python
@dataclass
class SitemapResult()
```

Parsed sitemap body.

`urls` holds the entries from a `<urlset>`; `sitemaps` holds child sitemap
URLs from a `<sitemapindex>` (each of which is fetched recursively).

<a id="scrapling.spiders.templates.sitemap.SitemapSpider"></a>

## SitemapSpider Objects

```python
class SitemapSpider(Spider)
```

A Spider that seeds a crawl from sitemap(s), and follows the rules.

Override `rules()` to return a list of `CrawlRule`s.

If there are no rules provided, all non-sitemap urls will be redirected to `parse()`, which must be overridden or it will raise `NotImplementedError`.

:cvar sitemap_urls: Explicit list of sitemap (or robots.txt) URLs to fetch.
:cvar sitemap_follow: `LinkExtractor` filtering which child sitemaps inside a
    `<sitemapindex>` to descend into. ``None`` means descend into all.
:cvar sitemap_alternate_links: When enabled, alternate-language URLs are also
    routed through `rules()`.

<a id="scrapling.spiders.templates.sitemap.SitemapSpider.rules"></a>

#### rules

```python
def rules() -> List[CrawlRule]
```

Override to define dispatch rules for sitemap URLs.

<a id="scrapling.spiders.templates.sitemap.SitemapSpider.parse"></a>

#### parse

```python
async def parse(
    response: "Response"
) -> AsyncGenerator[Union[Dict[str, Any], Request, None], None]
```

Default callback for processing responses

<a id="scrapling.spiders.templates"></a>

# scrapling.spiders.templates

<a id="scrapling.spiders.cache"></a>

# scrapling.spiders.cache

<a id="scrapling.spiders.cache.ResponseCacheManager"></a>

## ResponseCacheManager Objects

```python
class ResponseCacheManager()
```

Caches HTTP responses to disk for replay during spider development.

<a id="scrapling.spiders.scheduler"></a>

# scrapling.spiders.scheduler

<a id="scrapling.spiders.scheduler.Scheduler"></a>

## Scheduler Objects

```python
class Scheduler()
```

Priority queue with URL deduplication. (heapq)

Higher priority requests are processed first.
Duplicate URLs are filtered unless dont_filter=True.

<a id="scrapling.spiders.scheduler.Scheduler.enqueue"></a>

#### enqueue

```python
async def enqueue(request: Request) -> bool
```

Add a request to the queue.

<a id="scrapling.spiders.scheduler.Scheduler.dequeue"></a>

#### dequeue

```python
async def dequeue() -> Request
```

Get the next request to process.

<a id="scrapling.spiders.scheduler.Scheduler.snapshot"></a>

#### snapshot

```python
def snapshot() -> Tuple[List[Request], Set[bytes]]
```

Create a snapshot of the current state for checkpoints.

<a id="scrapling.spiders.scheduler.Scheduler.restore"></a>

#### restore

```python
def restore(data: "CheckpointData") -> None
```

Restore scheduler state from checkpoint data.

**Arguments**:

- `data`: CheckpointData containing requests and seen set

<a id="scrapling.spiders.request"></a>

# scrapling.spiders.request

<a id="scrapling.spiders.request.Request"></a>

## Request Objects

```python
class Request()
```

<a id="scrapling.spiders.request.Request.copy"></a>

#### copy

```python
def copy() -> "Request"
```

Create a copy of this request.

<a id="scrapling.spiders.request.Request.update_fingerprint"></a>

#### update\_fingerprint

```python
def update_fingerprint(include_kwargs: bool = False,
                       include_headers: bool = False,
                       keep_fragments: bool = False) -> bytes
```

Generate a unique fingerprint for deduplication.

Caches the result in self._fp after first computation.

<a id="scrapling.spiders.request.Request.__lt__"></a>

#### \_\_lt\_\_

```python
def __lt__(other: object) -> bool
```

Compare requests by priority

<a id="scrapling.spiders.request.Request.__gt__"></a>

#### \_\_gt\_\_

```python
def __gt__(other: object) -> bool
```

Compare requests by priority

<a id="scrapling.spiders.request.Request.__eq__"></a>

#### \_\_eq\_\_

```python
def __eq__(other: object) -> bool
```

Requests are equal if they have the same fingerprint.

<a id="scrapling.spiders.request.Request.__getstate__"></a>

#### \_\_getstate\_\_

```python
def __getstate__() -> dict[str, Any]
```

Prepare state for pickling - store callback as name string for pickle compatibility.

<a id="scrapling.spiders.request.Request.__setstate__"></a>

#### \_\_setstate\_\_

```python
def __setstate__(state: dict[str, Any]) -> None
```

Restore state from pickle - callback restored later via _restore_callback().

<a id="scrapling.spiders.checkpoint"></a>

# scrapling.spiders.checkpoint

<a id="scrapling.spiders.checkpoint.CheckpointData"></a>

## CheckpointData Objects

```python
@dataclass
class CheckpointData()
```

Container for checkpoint state.

<a id="scrapling.spiders.checkpoint.CheckpointManager"></a>

## CheckpointManager Objects

```python
class CheckpointManager()
```

Manages saving and loading checkpoint state to/from disk.

<a id="scrapling.spiders.checkpoint.CheckpointManager.has_checkpoint"></a>

#### has\_checkpoint

```python
async def has_checkpoint() -> bool
```

Check if a checkpoint exists.

<a id="scrapling.spiders.checkpoint.CheckpointManager.save"></a>

#### save

```python
async def save(data: CheckpointData) -> None
```

Save checkpoint data to disk atomically.

<a id="scrapling.spiders.checkpoint.CheckpointManager.load"></a>

#### load

```python
async def load() -> Optional[CheckpointData]
```

Load checkpoint data from disk.

Returns None if no checkpoint exists or if loading fails.

<a id="scrapling.spiders.checkpoint.CheckpointManager.cleanup"></a>

#### cleanup

```python
async def cleanup() -> None
```

Delete checkpoint file after successful completion.

<a id="scrapling.spiders.links"></a>

# scrapling.spiders.links

Pure URL discovery primitive

<a id="scrapling.spiders.links.LinkExtractor"></a>

## LinkExtractor Objects

```python
class LinkExtractor()
```

Extracts and filters URLs from a `Response` (or a single URL via `matches`).

All matching is regex-based; allow/deny patterns can be plain strings (compiled
with `re.compile`) or pre-compiled `re.Pattern` objects, individually or as an
iterable.

**Arguments**:

- `allow`: Regex pattern(s) URLs must match to be kept. String, compiled `re.Pattern`,
or an iterable of either. Empty means match all.
- `deny`: Regex pattern(s) URLs must NOT match. Takes precedence over `allow`.
- `allow_domains`: Domain(s) to keep. Matches the exact host or any subdomain
(e.g. `"example.com"` matches `"api.example.com"`). String or iterable.
- `deny_domains`: Domain(s) to exclude. Same matching rules as `allow_domains`.
- `restrict_css`: CSS selectors to scope DOM extraction to. Empty means whole page.
- `restrict_xpath`: XPath selectors to scope DOM extraction to. Empty means whole page.
- `tags`: Element tags to look for links in. Default ("a", "area").
- `attrs`: Attributes on those tags to read URLs from. Default ("href",).
- `canonicalize`: Canonicalize URLs (sort query params, normalize path). Default True.
- `strip`: Strip whitespace from extracted URLs. Default True.
- `keep_fragment`: Preserve the URL fragment when canonicalizing. Default False.
- `deny_extensions`: File extensions to drop. Default `IGNORED_EXTENSIONS`.
- `process`: A function to do a process on the values extracted before using them. Return None to drop any value.

<a id="scrapling.spiders.links.LinkExtractor.extract"></a>

#### extract

```python
def extract(response: "Response") -> List[str]
```

Return absolute, filtered, deduped URLs from `response`.

<a id="scrapling.spiders.links.LinkExtractor.matches"></a>

#### matches

```python
def matches(url: str) -> bool
```

URL-only filter (no response extraction).

Applies allow/deny/allow_domains/deny_domains/deny_extensions to a single URL.
Used by `SitemapSpider` to dispatch sitemap URLs through `CrawlRule`s without
needing a `Response`.

<a id="scrapling.spiders.engine"></a>

# scrapling.spiders.engine

<a id="scrapling.spiders.engine.CrawlerEngine"></a>

## CrawlerEngine Objects

```python
class CrawlerEngine()
```

Orchestrates the crawling process.

<a id="scrapling.spiders.engine.CrawlerEngine.request_pause"></a>

#### request\_pause

```python
def request_pause() -> None
```

Request a graceful pause of the crawl.

First call: requests graceful pause (waits for active tasks).
Second call: forces immediate stop.

<a id="scrapling.spiders.engine.CrawlerEngine.crawl"></a>

#### crawl

```python
async def crawl() -> CrawlStats
```

Run the spider and return CrawlStats.

<a id="scrapling.spiders.engine.CrawlerEngine.items"></a>

#### items

```python
@property
def items() -> ItemList
```

Access scraped items.

<a id="scrapling.spiders.spider"></a>

# scrapling.spiders.spider

<a id="scrapling.spiders.spider.LogCounterHandler"></a>

## LogCounterHandler Objects

```python
class LogCounterHandler(logging.Handler)
```

A logging handler that counts log messages by level.

<a id="scrapling.spiders.spider.LogCounterHandler.get_counts"></a>

#### get\_counts

```python
def get_counts() -> Dict[str, int]
```

Return counts as a dictionary with string keys.

<a id="scrapling.spiders.spider.SessionConfigurationError"></a>

## SessionConfigurationError Objects

```python
class SessionConfigurationError(Exception)
```

Raised when session configuration fails.

<a id="scrapling.spiders.spider.Spider"></a>

## Spider Objects

```python
class Spider(ABC)
```

An abstract base class for creating web spiders.

Check the documentation website for more information.

<a id="scrapling.spiders.spider.Spider.__init__"></a>

#### \_\_init\_\_

```python
def __init__(crawldir: Optional[Union[str, Path, AsyncPath]] = None,
             interval: float = 300.0)
```

Initialize the spider.

**Arguments**:

- `crawldir`: Directory for checkpoint files. If provided, enables pause/resume.
- `interval`: Seconds between periodic checkpoint saves (default 5 minutes).

<a id="scrapling.spiders.spider.Spider.start_requests"></a>

#### start\_requests

```python
async def start_requests() -> AsyncGenerator[Request, None]
```

Generate initial requests to start the crawl.

By default, this generates Request objects for each URL in `start_urls`
using the session manager's default session and `parse()` as callback.

Override this method for more control over initial requests
(e.g., to add custom headers, use different callbacks, etc.)

<a id="scrapling.spiders.spider.Spider.parse"></a>

#### parse

```python
@abstractmethod
async def parse(
    response: "Response"
) -> AsyncGenerator[Dict[str, Any] | Request | None, None]
```

Default callback for processing responses

<a id="scrapling.spiders.spider.Spider.on_start"></a>

#### on\_start

```python
async def on_start(resuming: bool = False) -> None
```

Called before crawling starts. Override for setup logic.

**Arguments**:

- `resuming`: It's enabled if the spider is resuming from a checkpoint, left for the user to use.

<a id="scrapling.spiders.spider.Spider.on_close"></a>

#### on\_close

```python
async def on_close() -> None
```

Called after crawling finishes. Override for cleanup logic.

<a id="scrapling.spiders.spider.Spider.on_error"></a>

#### on\_error

```python
async def on_error(request: Request, error: Exception) -> None
```

Handle request errors for all spider requests.

Override for custom error handling.

<a id="scrapling.spiders.spider.Spider.on_scraped_item"></a>

#### on\_scraped\_item

```python
async def on_scraped_item(item: Dict[str, Any]) -> Dict[str, Any] | None
```

A hook to be overridden by users to do some processing on scraped items, return `None` to drop the item silently.

<a id="scrapling.spiders.spider.Spider.is_blocked"></a>

#### is\_blocked

```python
async def is_blocked(response: "Response") -> bool
```

Check if the response is blocked. Users should override this for custom detection logic.

<a id="scrapling.spiders.spider.Spider.retry_blocked_request"></a>

#### retry\_blocked\_request

```python
async def retry_blocked_request(request: Request,
                                response: "Response") -> Request
```

Users should override this to prepare the blocked request before retrying, if needed.

<a id="scrapling.spiders.spider.Spider.__repr__"></a>

#### \_\_repr\_\_

```python
def __repr__() -> str
```

String representation of the spider.

<a id="scrapling.spiders.spider.Spider.configure_sessions"></a>

#### configure\_sessions

```python
def configure_sessions(manager: SessionManager) -> None
```

Configure sessions for this spider.

Override this method to add custom sessions.
The default implementation creates a FetcherSession session.

The first session added becomes the default for `start_requests()` unless specified otherwise.

**Arguments**:

- `manager`: SessionManager to configure

<a id="scrapling.spiders.spider.Spider.pause"></a>

#### pause

```python
def pause()
```

Request graceful shutdown of the crawling process.

<a id="scrapling.spiders.spider.Spider.start"></a>

#### start

```python
def start(use_uvloop: bool = False, **backend_options: Any) -> CrawlResult
```

Run the spider and return results.

This is the main entry point for running a spider.
Handles async execution internally via anyio.

Pressing Ctrl+C will initiate graceful shutdown (waits for active tasks to complete).
Pressing Ctrl+C a second time will force immediate stop.

If crawldir is set, a checkpoint will also be saved on graceful shutdown,
allowing you to resume the crawl later by running the spider again.

**Arguments**:

- `use_uvloop`: Whether to use the faster uvloop/winloop event loop implementation, if available.
- `backend_options`: Asyncio backend options to be used with `anyio.run`

<a id="scrapling.spiders.spider.Spider.stream"></a>

#### stream

```python
async def stream() -> AsyncGenerator[Dict[str, Any], None]
```

Stream items as they're scraped. Ideal for long-running spiders or building applications on top of the spiders.

Must be called from an async context. Yields items one by one as they are scraped.
Access `spider.stats` during iteration for real-time statistics.

Note: SIGINT handling for pause/resume is not available in stream mode.

<a id="scrapling.spiders.spider.Spider.stats"></a>

#### stats

```python
@property
def stats() -> CrawlStats
```

Access current crawl stats (works during streaming).

<a id="scrapling.spiders.result"></a>

# scrapling.spiders.result

<a id="scrapling.spiders.result.ItemList"></a>

## ItemList Objects

```python
class ItemList(list)
```

A list of scraped items with export capabilities.

<a id="scrapling.spiders.result.ItemList.to_json"></a>

#### to\_json

```python
def to_json(path: Union[str, Path], *, indent: bool = False)
```

Export items to a JSON file.

**Arguments**:

- `path`: Path to the output file
- `indent`: Pretty-print with 2-space indentation (slightly slower)

<a id="scrapling.spiders.result.ItemList.to_jsonl"></a>

#### to\_jsonl

```python
def to_jsonl(path: Union[str, Path])
```

Export items as JSON Lines (one JSON object per line).

**Arguments**:

- `path`: Path to the output file

<a id="scrapling.spiders.result.CrawlStats"></a>

## CrawlStats Objects

```python
@dataclass
class CrawlStats()
```

Statistics for a crawl run.

<a id="scrapling.spiders.result.CrawlResult"></a>

## CrawlResult Objects

```python
@dataclass
class CrawlResult()
```

Complete result from a spider run.

<a id="scrapling.spiders.result.CrawlResult.completed"></a>

#### completed

```python
@property
def completed() -> bool
```

True if the crawl completed normally (not paused).

<a id="scrapling.spiders"></a>

# scrapling.spiders

<a id="scrapling.cli"></a>

# scrapling.cli

<a id="scrapling.cli.extract"></a>

#### extract

```python
@group(
    help=
    "Fetch web pages using various fetchers and extract full/selected HTML content as HTML, Markdown, or extract text content."
)
def extract()
```

Extract content from web pages and save to files

<a id="scrapling.cli.get"></a>

#### get

```python
@extract.command(
    help=
    f"Perform a GET request and save the content to a file.\n\n{__OUTPUT_FILE_HELP__}"
)
@argument("url", required=True)
@argument("output_file", required=True)
@_common_http_options
def get(url, output_file, headers, cookies, timeout, proxy, css_selector,
        params, follow_redirects, verify, impersonate, stealthy_headers,
        ai_targeted)
```

Perform a GET request and save the content to a file.

<a id="scrapling.cli.post"></a>

#### post

```python
@extract.command(
    help=
    f"Perform a POST request and save the content to a file.\n\n{__OUTPUT_FILE_HELP__}"
)
@argument("url", required=True)
@argument("output_file", required=True)
@_data_options
@_common_http_options
def post(url, output_file, data, json, headers, cookies, timeout, proxy,
         css_selector, params, follow_redirects, verify, impersonate,
         stealthy_headers, ai_targeted)
```

Perform a POST request and save the content to a file.

<a id="scrapling.cli.put"></a>

#### put

```python
@extract.command(
    help=
    f"Perform a PUT request and save the content to a file.\n\n{__OUTPUT_FILE_HELP__}"
)
@argument("url", required=True)
@argument("output_file", required=True)
@_data_options
@_common_http_options
def put(url, output_file, data, json, headers, cookies, timeout, proxy,
        css_selector, params, follow_redirects, verify, impersonate,
        stealthy_headers, ai_targeted)
```

Perform a PUT request and save the content to a file.

<a id="scrapling.cli.delete"></a>

#### delete

```python
@extract.command(
    help=
    f"Perform a DELETE request and save the content to a file.\n\n{__OUTPUT_FILE_HELP__}"
)
@argument("url", required=True)
@argument("output_file", required=True)
@_common_http_options
def delete(url, output_file, headers, cookies, timeout, proxy, css_selector,
           params, follow_redirects, verify, impersonate, stealthy_headers,
           ai_targeted)
```

Perform a DELETE request and save the content to a file.

<a id="scrapling.cli.fetch"></a>

#### fetch

```python
@extract.command(
    help=
    f"Use DynamicFetcher to fetch content with browser automation.\n\n{__OUTPUT_FILE_HELP__}"
)
@argument("url", required=True)
@argument("output_file", required=True)
@_common_browser_options
def fetch(url, output_file, headless, disable_resources, network_idle, timeout,
          wait, css_selector, wait_selector, locale, real_chrome, proxy,
          extra_headers, ai_targeted, dns_over_https, block_ads)
```

Opens up a browser and fetch content using DynamicFetcher.

<a id="scrapling.cli.stealthy_fetch"></a>

#### stealthy\_fetch

```python
@extract.command(
    help=
    f"Use StealthyFetcher to fetch content with advanced stealth features.\n\n{__OUTPUT_FILE_HELP__}"
)
@argument("url", required=True)
@argument("output_file", required=True)
@option(
    "--block-webrtc/--allow-webrtc",
    default=False,
    help="Block WebRTC entirely (default: False)",
)
@option(
    "--solve-cloudflare/--no-solve-cloudflare",
    default=False,
    help="Solve Cloudflare challenges (default: False)",
)
@option("--allow-webgl/--block-webgl",
        default=True,
        help="Allow WebGL (default: True)")
@option(
    "--hide-canvas/--show-canvas",
    default=False,
    help="Add noise to canvas operations (default: False)",
)
@_common_browser_options
def stealthy_fetch(url, output_file, headless, disable_resources, network_idle,
                   timeout, wait, css_selector, wait_selector, locale,
                   real_chrome, proxy, extra_headers, block_webrtc,
                   solve_cloudflare, allow_webgl, hide_canvas, ai_targeted,
                   dns_over_https, block_ads)
```

Opens up a browser with advanced stealth features and fetch content using StealthyFetcher.

<a id="scrapling.core.ai"></a>

# scrapling.core.ai

<a id="scrapling.core.ai.ResponseModel"></a>

## ResponseModel Objects

```python
class ResponseModel(BaseModel)
```

Request's response information structure.

<a id="scrapling.core.ai.SessionInfo"></a>

## SessionInfo Objects

```python
class SessionInfo(BaseModel)
```

Information about an open browser session.

<a id="scrapling.core.ai.SessionCreatedModel"></a>

## SessionCreatedModel Objects

```python
class SessionCreatedModel(SessionInfo)
```

Response returned when a new session is created.

<a id="scrapling.core.ai.SessionClosedModel"></a>

## SessionClosedModel Objects

```python
class SessionClosedModel(BaseModel)
```

Response returned when a session is closed.

<a id="scrapling.core.ai._SessionEntry"></a>

## \_SessionEntry Objects

```python
@dataclass
class _SessionEntry()
```

<a id="scrapling.core.ai._SessionEntry.session"></a>

#### session

AsyncDynamicSession | AsyncStealthySession

<a id="scrapling.core.ai.ScraplingMCPServer"></a>

## ScraplingMCPServer Objects

```python
class ScraplingMCPServer()
```

<a id="scrapling.core.ai.ScraplingMCPServer.open_session"></a>

#### open\_session

```python
async def open_session(
        session_type: SessionType,
        session_id: Optional[str] = None,
        headless: bool = True,
        google_search: bool = True,
        real_chrome: bool = False,
        wait: int | float = 0,
        proxy: Optional[str | Dict[str, str]] = None,
        timezone_id: str | None = None,
        locale: str | None = None,
        extra_headers: Optional[Dict[str, str]] = None,
        useragent: Optional[str] = None,
        cdp_url: Optional[str] = None,
        timeout: int | float = 30000,
        disable_resources: bool = False,
        wait_selector: Optional[str] = None,
        cookies: Sequence[SetCookieParam] | None = None,
        network_idle: bool = False,
        wait_selector_state: SelectorWaitStates = "attached",
        max_pages: int = 5,
        hide_canvas: bool = False,
        block_webrtc: bool = False,
        allow_webgl: bool = True,
        solve_cloudflare: bool = False,
        additional_args: Optional[Dict] = None) -> SessionCreatedModel
```

Open a persistent browser session that can be reused across multiple fetch calls.

This avoids the overhead of launching a new browser for each request.
Use close_session to close the session when done, and list_sessions to see all active sessions.

**Arguments**:

- `session_type`: The type of session to open. Use "dynamic" for standard Playwright browser, or "stealthy" for anti-bot bypass with fingerprint spoofing.
- `session_id`: Optional custom session ID. If not provided, a random 12-character hex ID will be generated. Useful for naming sessions for easier management.
- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the Response object.
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc.
- `extra_headers`: A dictionary of extra headers to add to the request.
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `cookies`: Set cookies for the session. It should be in a dictionary format that Playwright accepts.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `max_pages`: Maximum number of concurrent pages/tabs in the browser. Defaults to 5. Higher values allow more parallel fetches.
- `hide_canvas`: (Stealthy only) Add random noise to canvas operations to prevent fingerprinting.
- `block_webrtc`: (Stealthy only) Forces WebRTC to respect proxy settings to prevent local IP address leak.
- `allow_webgl`: (Stealthy only) Enabled by default. Disabling WebGL is not recommended as many WAFs now check if WebGL is enabled.
- `solve_cloudflare`: (Stealthy only) Solves all types of the Cloudflare's Turnstile/Interstitial challenges.
- `additional_args`: (Stealthy only) Additional arguments to be passed to Playwright's context as additional settings.

<a id="scrapling.core.ai.ScraplingMCPServer.close_session"></a>

#### close\_session

```python
async def close_session(session_id: str) -> SessionClosedModel
```

Close a persistent browser session and free its resources.

**Arguments**:

- `session_id`: The unique identifier of the session to close. Use list_sessions to see active sessions.

<a id="scrapling.core.ai.ScraplingMCPServer.list_sessions"></a>

#### list\_sessions

```python
async def list_sessions() -> List[SessionInfo]
```

List all active browser sessions with their details.

<a id="scrapling.core.ai.ScraplingMCPServer.screenshot"></a>

#### screenshot

```python
async def screenshot(
        url: str,
        session_id: str,
        image_type: ScreenshotType = "png",
        full_page: bool = False,
        quality: Optional[int] = None,
        wait: int | float = 0,
        wait_selector: Optional[str] = None,
        wait_selector_state: SelectorWaitStates = "attached",
        network_idle: bool = False,
        timeout: int | float = 30000) -> List[ImageContent | TextContent]
```

Capture a screenshot of a web page using an existing browser session and return it as an image.

A browser session must be opened first with `open_session` (either `dynamic` or `stealthy`); the session ID is then passed here.

**Arguments**:

- `url`: The URL to navigate to and capture.
- `session_id`: ID of an open browser session created with `open_session`.
- `image_type`: Image format. Defaults to "png". Use "jpeg" for smaller file sizes.
- `full_page`: When True, captures the full scrollable page instead of just the viewport. Defaults to False.
- `quality`: Image quality (0-100) for JPEG only. Raises if passed with `image_type="png"`.
- `wait`: Time in milliseconds to wait after page load before capturing. Defaults to 0.
- `wait_selector`: Optional CSS selector to wait for before capturing.
- `wait_selector_state`: State to wait for the selector. Defaults to "attached".
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `timeout`: Timeout in milliseconds for page operations. Defaults to 30,000.

<a id="scrapling.core.ai.ScraplingMCPServer.get"></a>

#### get

```python
@staticmethod
async def get(url: str,
              impersonate: ImpersonateType = "chrome",
              extraction_type: extraction_types = "markdown",
              css_selector: Optional[str] = None,
              main_content_only: bool = True,
              params: Optional[Dict] = None,
              headers: Optional[Mapping[str, Optional[str]]] = None,
              cookies: Optional[Dict[str, str]] = None,
              timeout: Optional[int | float] = 30,
              follow_redirects: FollowRedirects = "safe",
              max_redirects: int = 30,
              retries: Optional[int] = 3,
              retry_delay: Optional[int] = 1,
              proxy: Optional[str] = None,
              proxy_auth: Optional[Dict[str, str]] = None,
              auth: Optional[Dict[str, str]] = None,
              verify: Optional[bool] = True,
              http3: Optional[bool] = False,
              stealthy_headers: Optional[bool] = True) -> ResponseModel
```

Make GET HTTP request to a URL and return a structured output of the result.

Note: This is only suitable for low-mid protection levels. For high-protection levels or websites that require JS loading, use the other tools directly.
Note: If the `css_selector` resolves to more than one element, all the elements will be returned.

**Arguments**:

- `url`: The URL to request.
- `impersonate`: Browser version to impersonate its fingerprint. It's using the latest chrome version by default.
- `extraction_type`: The type of content to extract from the page. Defaults to "markdown". Options are:
- Markdown will convert the page content to Markdown format.
- HTML will return the raw HTML content of the page.
- Text will return the text content of the page.
- `css_selector`: CSS selector to extract the content from the page. If main_content_only is True, then it will be executed on the main content of the page. Defaults to None.
- `main_content_only`: Whether to extract only the main content of the page. Defaults to True. The main content here is the data inside the `<body>` tag.
- `params`: Query string parameters for the request.
- `headers`: Headers to include in the request.
- `cookies`: Cookies to use in the request.
- `timeout`: Number of seconds to wait before timing out.
- `follow_redirects`: Whether to follow redirects. Defaults to "safe", which follows redirects but rejects those targeting internal/private IPs (SSRF protection).
Pass True to follow all redirects without restriction.
- `max_redirects`: Maximum number of redirects. Default 30, use -1 for unlimited.
- `retries`: Number of retry attempts. Defaults to 3.
- `retry_delay`: Number of seconds to wait between retry attempts. Defaults to 1 second.
- `proxy`: Proxy URL to use. Format: "http://username:password@localhost:8030".
Cannot be used together with the `proxies` parameter.
- `proxy_auth`: HTTP basic auth for proxy in dictionary format with `username` and `password` keys.
- `auth`: HTTP basic auth in dictionary format with `username` and `password` keys.
- `verify`: Whether to verify HTTPS certificates.
- `http3`: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- `stealthy_headers`: If enabled (default), it creates and adds real browser headers. It also sets a Google referer header.

<a id="scrapling.core.ai.ScraplingMCPServer.bulk_get"></a>

#### bulk\_get

```python
@staticmethod
async def bulk_get(
        urls: List[str],
        impersonate: ImpersonateType = "chrome",
        extraction_type: extraction_types = "markdown",
        css_selector: Optional[str] = None,
        main_content_only: bool = True,
        params: Optional[Dict] = None,
        headers: Optional[Mapping[str, Optional[str]]] = None,
        cookies: Optional[Dict[str, str]] = None,
        timeout: Optional[int | float] = 30,
        follow_redirects: FollowRedirects = "safe",
        max_redirects: int = 30,
        retries: Optional[int] = 3,
        retry_delay: Optional[int] = 1,
        proxy: Optional[str] = None,
        proxy_auth: Optional[Dict[str, str]] = None,
        auth: Optional[Dict[str, str]] = None,
        verify: Optional[bool] = True,
        http3: Optional[bool] = False,
        stealthy_headers: Optional[bool] = True) -> List[ResponseModel]
```

Make GET HTTP request to a group of URLs and for each URL, return a structured output of the result.

Note: This is only suitable for low-mid protection levels. For high-protection levels or websites that require JS loading, use the other tools directly.
Note: If the `css_selector` resolves to more than one element, all the elements will be returned.

**Arguments**:

- `urls`: A list of the URLs to request.
- `impersonate`: Browser version to impersonate its fingerprint. It's using the latest chrome version by default.
- `extraction_type`: The type of content to extract from the page. Defaults to "markdown". Options are:
- Markdown will convert the page content to Markdown format.
- HTML will return the raw HTML content of the page.
- Text will return the text content of the page.
- `css_selector`: CSS selector to extract the content from the page. If main_content_only is True, then it will be executed on the main content of the page. Defaults to None.
- `main_content_only`: Whether to extract only the main content of the page. Defaults to True. The main content here is the data inside the `<body>` tag.
- `params`: Query string parameters for the request.
- `headers`: Headers to include in the request.
- `cookies`: Cookies to use in the request.
- `timeout`: Number of seconds to wait before timing out.
- `follow_redirects`: Whether to follow redirects. Defaults to "safe", which follows redirects but rejects those targeting internal/private IPs (SSRF protection).
Pass True to follow all redirects without restriction.
- `max_redirects`: Maximum number of redirects. Default 30, use -1 for unlimited.
- `retries`: Number of retry attempts. Defaults to 3.
- `retry_delay`: Number of seconds to wait between retry attempts. Defaults to 1 second.
- `proxy`: Proxy URL to use. Format: "http://username:password@localhost:8030".
Cannot be used together with the `proxies` parameter.
- `proxy_auth`: HTTP basic auth for proxy in dictionary format with `username` and `password` keys.
- `auth`: HTTP basic auth in dictionary format with `username` and `password` keys.
- `verify`: Whether to verify HTTPS certificates.
- `http3`: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- `stealthy_headers`: If enabled (default), it creates and adds real browser headers. It also sets a Google referer header.

<a id="scrapling.core.ai.ScraplingMCPServer.fetch"></a>

#### fetch

```python
async def fetch(url: str,
                extraction_type: extraction_types = "markdown",
                css_selector: Optional[str] = None,
                main_content_only: bool = True,
                headless: bool = True,
                google_search: bool = True,
                real_chrome: bool = False,
                wait: int | float = 0,
                proxy: Optional[str | Dict[str, str]] = None,
                timezone_id: str | None = None,
                locale: str | None = None,
                extra_headers: Optional[Dict[str, str]] = None,
                useragent: Optional[str] = None,
                cdp_url: Optional[str] = None,
                timeout: int | float = 30000,
                disable_resources: bool = False,
                wait_selector: Optional[str] = None,
                cookies: Sequence[SetCookieParam] | None = None,
                network_idle: bool = False,
                wait_selector_state: SelectorWaitStates = "attached",
                session_id: Optional[str] = None) -> ResponseModel
```

Use playwright to open a browser to fetch a URL and return a structured output of the result.

Note: This is only suitable for low-mid protection levels.
Note: If the `css_selector` resolves to more than one element, all the elements will be returned.
Note: If a `session_id` is provided (from open_session), the browser session will be reused instead of creating a new one.
    When using a session, browser-level params (headless, proxy, locale, etc.) are ignored since they were set at session creation time.

**Arguments**:

- `url`: The URL to request.
- `extraction_type`: The type of content to extract from the page. Defaults to "markdown". Options are:
- Markdown will convert the page content to Markdown format.
- HTML will return the raw HTML content of the page.
- Text will return the text content of the page.
- `css_selector`: CSS selector to extract the content from the page. If main_content_only is True, then it will be executed on the main content of the page. Defaults to None.
- `main_content_only`: Whether to extract only the main content of the page. Defaults to True. The main content here is the data inside the `<body>` tag.
- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request. It should be in a dictionary format that Playwright accepts.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `session_id`: Optional session ID from open_session. If provided, reuses the existing browser session instead of creating a new one.

<a id="scrapling.core.ai.ScraplingMCPServer.bulk_fetch"></a>

#### bulk\_fetch

```python
async def bulk_fetch(urls: List[str],
                     extraction_type: extraction_types = "markdown",
                     css_selector: Optional[str] = None,
                     main_content_only: bool = True,
                     headless: bool = True,
                     google_search: bool = True,
                     real_chrome: bool = False,
                     wait: int | float = 0,
                     proxy: Optional[str | Dict[str, str]] = None,
                     timezone_id: str | None = None,
                     locale: str | None = None,
                     extra_headers: Optional[Dict[str, str]] = None,
                     useragent: Optional[str] = None,
                     cdp_url: Optional[str] = None,
                     timeout: int | float = 30000,
                     disable_resources: bool = False,
                     wait_selector: Optional[str] = None,
                     cookies: Sequence[SetCookieParam] | None = None,
                     network_idle: bool = False,
                     wait_selector_state: SelectorWaitStates = "attached",
                     session_id: Optional[str] = None) -> List[ResponseModel]
```

Use playwright to open a browser, then fetch a group of URLs at the same time, and for each page return a structured output of the result.

Note: This is only suitable for low-mid protection levels.
Note: If the `css_selector` resolves to more than one element, all the elements will be returned.
Note: If a `session_id` is provided (from open_session), the browser session will be reused instead of creating a new one.
    When using a session, browser-level params (headless, proxy, locale, etc.) are ignored since they were set at session creation time.

**Arguments**:

- `urls`: A list of the URLs to request.
- `extraction_type`: The type of content to extract from the page. Defaults to "markdown". Options are:
- Markdown will convert the page content to Markdown format.
- HTML will return the raw HTML content of the page.
- Text will return the text content of the page.
- `css_selector`: CSS selector to extract the content from the page. If main_content_only is True, then it will be executed on the main content of the page. Defaults to None.
- `main_content_only`: Whether to extract only the main content of the page. Defaults to True. The main content here is the data inside the `<body>` tag.
- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request. It should be in a dictionary format that Playwright accepts.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `session_id`: Optional session ID from open_session. If provided, reuses the existing browser session instead of creating a new one.

<a id="scrapling.core.ai.ScraplingMCPServer.stealthy_fetch"></a>

#### stealthy\_fetch

```python
async def stealthy_fetch(url: str,
                         extraction_type: extraction_types = "markdown",
                         css_selector: Optional[str] = None,
                         main_content_only: bool = True,
                         headless: bool = True,
                         google_search: bool = True,
                         real_chrome: bool = False,
                         wait: int | float = 0,
                         proxy: Optional[str | Dict[str, str]] = None,
                         timezone_id: str | None = None,
                         locale: str | None = None,
                         extra_headers: Optional[Dict[str, str]] = None,
                         useragent: Optional[str] = None,
                         hide_canvas: bool = False,
                         cdp_url: Optional[str] = None,
                         timeout: int | float = 30000,
                         disable_resources: bool = False,
                         wait_selector: Optional[str] = None,
                         cookies: Sequence[SetCookieParam] | None = None,
                         network_idle: bool = False,
                         wait_selector_state: SelectorWaitStates = "attached",
                         block_webrtc: bool = False,
                         allow_webgl: bool = True,
                         solve_cloudflare: bool = False,
                         additional_args: Optional[Dict] = None,
                         session_id: Optional[str] = None) -> ResponseModel
```

Use the stealthy fetcher to fetch a URL and return a structured output of the result.

Note: This is the only suitable fetcher for high protection levels.
Note: If the `css_selector` resolves to more than one element, all the elements will be returned.
Note: If a `session_id` is provided (from open_session), the browser session will be reused instead of creating a new one.
    When using a session, browser-level params (headless, proxy, locale, etc.) are ignored since they were set at session creation time.

**Arguments**:

- `url`: The URL to request.
- `extraction_type`: The type of content to extract from the page. Defaults to "markdown". Options are:
- Markdown will convert the page content to Markdown format.
- HTML will return the raw HTML content of the page.
- Text will return the text content of the page.
- `css_selector`: CSS selector to extract the content from the page. If main_content_only is True, then it will be executed on the main content of the page. Defaults to None.
- `main_content_only`: Whether to extract only the main content of the page. Defaults to True. The main content here is the data inside the `<body>` tag.
- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `solve_cloudflare`: Solves all types of the Cloudflare's Turnstile/Interstitial challenges before returning the response to you.
- `allow_webgl`: Enabled by default. Disabling WebGL is not recommended as many WAFs now check if WebGL is enabled.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `hide_canvas`: Add random noise to canvas operations to prevent fingerprinting.
- `block_webrtc`: Forces WebRTC to respect proxy settings to prevent local IP address leak.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings, and it takes higher priority than Scrapling's settings.
- `session_id`: Optional session ID from open_session. If provided, reuses the existing browser session instead of creating a new one.

<a id="scrapling.core.ai.ScraplingMCPServer.bulk_stealthy_fetch"></a>

#### bulk\_stealthy\_fetch

```python
async def bulk_stealthy_fetch(
        urls: List[str],
        extraction_type: extraction_types = "markdown",
        css_selector: Optional[str] = None,
        main_content_only: bool = True,
        headless: bool = True,
        google_search: bool = True,
        real_chrome: bool = False,
        wait: int | float = 0,
        proxy: Optional[str | Dict[str, str]] = None,
        timezone_id: str | None = None,
        locale: str | None = None,
        extra_headers: Optional[Dict[str, str]] = None,
        useragent: Optional[str] = None,
        hide_canvas: bool = False,
        cdp_url: Optional[str] = None,
        timeout: int | float = 30000,
        disable_resources: bool = False,
        wait_selector: Optional[str] = None,
        cookies: Sequence[SetCookieParam] | None = None,
        network_idle: bool = False,
        wait_selector_state: SelectorWaitStates = "attached",
        block_webrtc: bool = False,
        allow_webgl: bool = True,
        solve_cloudflare: bool = False,
        additional_args: Optional[Dict] = None,
        session_id: Optional[str] = None) -> List[ResponseModel]
```

Use the stealthy fetcher to fetch a group of URLs at the same time, and for each page return a structured output of the result.

Note: This is the only suitable fetcher for high protection levels.
Note: If the `css_selector` resolves to more than one element, all the elements will be returned.
Note: If a `session_id` is provided (from open_session), the browser session will be reused instead of creating a new one.
    When using a session, browser-level params (headless, proxy, locale, etc.) are ignored since they were set at session creation time.

**Arguments**:

- `urls`: A list of the URLs to request.
- `extraction_type`: The type of content to extract from the page. Defaults to "markdown". Options are:
- Markdown will convert the page content to Markdown format.
- HTML will return the raw HTML content of the page.
- Text will return the text content of the page.
- `css_selector`: CSS selector to extract the content from the page. If main_content_only is True, then it will be executed on the main content of the page. Defaults to None.
- `main_content_only`: Whether to extract only the main content of the page. Defaults to True. The main content here is the data inside the `<body>` tag.
- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `solve_cloudflare`: Solves all types of the Cloudflare's Turnstile/Interstitial challenges before returning the response to you.
- `allow_webgl`: Enabled by default. Disabling WebGL is not recommended as many WAFs now check if WebGL is enabled.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `hide_canvas`: Add random noise to canvas operations to prevent fingerprinting.
- `block_webrtc`: Forces WebRTC to respect proxy settings to prevent local IP address leak.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings, and it takes higher priority than Scrapling's settings.
- `session_id`: Optional session ID from open_session. If provided, reuses the existing browser session instead of creating a new one.

<a id="scrapling.core.ai.ScraplingMCPServer.serve"></a>

#### serve

```python
def serve(http: bool, host: str, port: int)
```

Serve the MCP server.

<a id="scrapling.core.utils._utils"></a>

# scrapling.core.utils.\_utils

<a id="scrapling.core.utils._utils.setup_logger"></a>

#### setup\_logger

```python
@lru_cache(1, typed=True)
def setup_logger()
```

Create and configure a logger with a standard format.

**Returns**:

logging.Logger: Configured logger instance

<a id="scrapling.core.utils._utils.set_logger"></a>

#### set\_logger

```python
def set_logger(logger: logging.Logger) -> Token
```

Set the current context logger. Returns token for reset.

<a id="scrapling.core.utils._utils.reset_logger"></a>

#### reset\_logger

```python
def reset_logger(token: Token) -> None
```

Reset logger to previous state using token.

<a id="scrapling.core.utils._shell"></a>

# scrapling.core.utils.\_shell

<a id="scrapling.core.utils"></a>

# scrapling.core.utils

<a id="scrapling.core.shell"></a>

# scrapling.core.shell

<a id="scrapling.core.shell.CurlParser"></a>

## CurlParser Objects

```python
class CurlParser()
```

Builds the argument parser for relevant curl flags from DevTools.

<a id="scrapling.core.shell.CurlParser.parse"></a>

#### parse

```python
def parse(curl_command: str) -> Optional[Request]
```

Parses the curl command string into a structured context for Fetcher.

<a id="scrapling.core.shell.CustomShell"></a>

## CustomShell Objects

```python
class CustomShell()
```

A custom IPython shell with minimal dependencies

<a id="scrapling.core.shell.CustomShell.init_components"></a>

#### init\_components

```python
def init_components()
```

Initialize application components

<a id="scrapling.core.shell.CustomShell.banner"></a>

#### banner

```python
@staticmethod
def banner()
```

Create a custom banner for the shell

<a id="scrapling.core.shell.CustomShell.update_page"></a>

#### update\_page

```python
def update_page(result)
```

Update the current page and add to pages history

<a id="scrapling.core.shell.CustomShell.create_wrapper"></a>

#### create\_wrapper

```python
def create_wrapper(func: Callable,
                   get_signature: bool = True,
                   signature_name: Optional[str] = None) -> Callable
```

Create a wrapper that preserves function signature but updates page

<a id="scrapling.core.shell.CustomShell.get_namespace"></a>

#### get\_namespace

```python
def get_namespace()
```

Create a namespace with application-specific objects

<a id="scrapling.core.shell.CustomShell.show_help"></a>

#### show\_help

```python
def show_help()
```

Show help information

<a id="scrapling.core.shell.CustomShell.start"></a>

#### start

```python
def start()
```

Start the interactive shell

<a id="scrapling.core.shell.Convertor"></a>

## Convertor Objects

```python
class Convertor()
```

Utils for the extract shell command

<a id="scrapling.core.shell.Convertor.write_content_to_file"></a>

#### write\_content\_to\_file

```python
@classmethod
def write_content_to_file(cls,
                          page: Selector,
                          filename: str,
                          css_selector: Optional[str] = None,
                          main_content_only: bool = False) -> None
```

Write a Selector's content to a file

<a id="scrapling.core.custom_types"></a>

# scrapling.core.custom\_types

<a id="scrapling.core.custom_types.TextHandler"></a>

## TextHandler Objects

```python
class TextHandler(str)
```

Extends standard Python string by adding more functionality

<a id="scrapling.core.custom_types.TextHandler.sort"></a>

#### sort

```python
def sort(reverse: bool = False) -> Union[str, "TextHandler"]
```

Return a sorted version of the string

<a id="scrapling.core.custom_types.TextHandler.clean"></a>

#### clean

```python
def clean(remove_entities=False) -> Union[str, "TextHandler"]
```

Return a new version of the string after removing all white spaces and consecutive spaces

<a id="scrapling.core.custom_types.TextHandler.json"></a>

#### json

```python
def json() -> Dict
```

Return JSON response if the response is jsonable otherwise throw error

<a id="scrapling.core.custom_types.TextHandler.re"></a>

#### re

```python
def re(regex: str | Pattern,
       replace_entities: bool = True,
       clean_match: bool = False,
       case_sensitive: bool = True,
       check_match: bool = False) -> Union["TextHandlers", bool]
```

Apply the given regex to the current text and return a list of strings with the matches.

**Arguments**:

- `regex`: Can be either a compiled regular expression or a string.
- `replace_entities`: If enabled character entity references are replaced by their corresponding character
- `clean_match`: If enabled, this will ignore all whitespaces and consecutive spaces while matching
- `case_sensitive`: If disabled, function will set the regex to ignore the letters-case while compiling it
- `check_match`: Used to quickly check if this regex matches or not without any operations on the results

<a id="scrapling.core.custom_types.TextHandler.re_first"></a>

#### re\_first

```python
def re_first(regex: str | Pattern,
             default: Any = None,
             replace_entities: bool = True,
             clean_match: bool = False,
             case_sensitive: bool = True) -> "TextHandler"
```

Apply the given regex to text and return the first match if found, otherwise return the default value.

**Arguments**:

- `regex`: Can be either a compiled regular expression or a string.
- `default`: The default value to be returned if there is no match
- `replace_entities`: If enabled character entity references are replaced by their corresponding character
- `clean_match`: If enabled, this will ignore all whitespaces and consecutive spaces while matching
- `case_sensitive`: If disabled, function will set the regex to ignore the letters-case while compiling it

<a id="scrapling.core.custom_types.TextHandlers"></a>

## TextHandlers Objects

```python
class TextHandlers(List[TextHandler])
```

The :class:`TextHandlers` class is a subclass of the builtin ``List`` class, which provides a few additional methods.

<a id="scrapling.core.custom_types.TextHandlers.re"></a>

#### re

```python
def re(regex: str | Pattern,
       replace_entities: bool = True,
       clean_match: bool = False,
       case_sensitive: bool = True) -> "TextHandlers"
```

Call the ``.re()`` method for each element in this list and return

their results flattened as TextHandlers.

**Arguments**:

- `regex`: Can be either a compiled regular expression or a string.
- `replace_entities`: If enabled character entity references are replaced by their corresponding character
- `clean_match`: if enabled, this will ignore all whitespaces and consecutive spaces while matching
- `case_sensitive`: if disabled, the function will set the regex to ignore the letters-case while compiling it

<a id="scrapling.core.custom_types.TextHandlers.re_first"></a>

#### re\_first

```python
def re_first(regex: str | Pattern,
             default: Any = None,
             replace_entities: bool = True,
             clean_match: bool = False,
             case_sensitive: bool = True) -> TextHandler
```

Call the ``.re_first()`` method for each element in this list and return

the first result or the default value otherwise.

**Arguments**:

- `regex`: Can be either a compiled regular expression or a string.
- `default`: The default value to be returned if there is no match
- `replace_entities`: If enabled character entity references are replaced by their corresponding character
- `clean_match`: If enabled, this will ignore all whitespaces and consecutive spaces while matching
- `case_sensitive`: If disabled, function will set the regex to ignore the letters-case while compiling it

<a id="scrapling.core.custom_types.TextHandlers.get"></a>

#### get

```python
def get(default=None)
```

Returns the first item of the current list

**Arguments**:

- `default`: the default value to return if the current list is empty

<a id="scrapling.core.custom_types.AttributesHandler"></a>

## AttributesHandler Objects

```python
class AttributesHandler(Mapping[str, _TextHandlerType])
```

A read-only mapping to use instead of the standard dictionary for the speed boost, but at the same time I use it to add more functionalities.
If the standard dictionary is needed, convert this class to a dictionary with the `dict` function

<a id="scrapling.core.custom_types.AttributesHandler.get"></a>

#### get

```python
def get(key: str, default: Any = None) -> _TextHandlerType
```

Acts like the standard dictionary `.get()` method

<a id="scrapling.core.custom_types.AttributesHandler.search_values"></a>

#### search\_values

```python
def search_values(
        keyword: str,
        partial: bool = False) -> Generator["AttributesHandler", None, None]
```

Search current attributes by values and return a dictionary of each matching item

**Arguments**:

- `keyword`: The keyword to search for in the attribute values
- `partial`: If True, the function will search if keyword in each value instead of perfect match

<a id="scrapling.core.custom_types.AttributesHandler.json_string"></a>

#### json\_string

```python
@property
def json_string() -> bytes
```

Convert current attributes to JSON bytes if the attributes are JSON serializable otherwise throws error

<a id="scrapling.core.storage"></a>

# scrapling.core.storage

<a id="scrapling.core.storage.StorageSystemMixin"></a>

## StorageSystemMixin Objects

```python
class StorageSystemMixin(ABC)
```

<a id="scrapling.core.storage.StorageSystemMixin.__init__"></a>

#### \_\_init\_\_

```python
def __init__(url: Optional[str] = None)
```

**Arguments**:

- `url`: URL of the website we are working on to separate it from other websites data

<a id="scrapling.core.storage.StorageSystemMixin.save"></a>

#### save

```python
@abstractmethod
def save(element: HtmlElement, identifier: str) -> None
```

Saves the element's unique properties to the storage for retrieval and relocation later

**Arguments**:

- `element`: The element itself which we want to save to storage.
- `identifier`: This is the identifier that will be used to retrieve the element later from the storage. See
the docs for more info.

<a id="scrapling.core.storage.StorageSystemMixin.retrieve"></a>

#### retrieve

```python
@abstractmethod
def retrieve(identifier: str) -> Optional[Dict]
```

Using the identifier, we search the storage and return the unique properties of the element

**Arguments**:

- `identifier`: This is the identifier that will be used to retrieve the element from the storage. See
the docs for more info.

**Returns**:

A dictionary of the unique properties

<a id="scrapling.core.storage.SQLiteStorageSystem"></a>

## SQLiteStorageSystem Objects

```python
@lru_cache(1, typed=True)
class SQLiteStorageSystem(StorageSystemMixin)
```

The recommended system to use, it's race condition safe and thread safe.
Mainly built, so the library can run in threaded frameworks like scrapy or threaded tools
> It's optimized for threaded applications, but running it without threads shouldn't make it slow.

<a id="scrapling.core.storage.SQLiteStorageSystem.__init__"></a>

#### \_\_init\_\_

```python
def __init__(storage_file: str, url: Optional[str] = None)
```

**Arguments**:

- `storage_file`: File to be used to store elements' data.
- `url`: URL of the website we are working on to separate it from other websites data

<a id="scrapling.core.storage.SQLiteStorageSystem.save"></a>

#### save

```python
def save(element: HtmlElement, identifier: str) -> None
```

Saves the elements unique properties to the storage for retrieval and relocation later

**Arguments**:

- `element`: The element itself which we want to save to storage.
- `identifier`: This is the identifier that will be used to retrieve the element later from the storage. See
the docs for more info.

<a id="scrapling.core.storage.SQLiteStorageSystem.retrieve"></a>

#### retrieve

```python
def retrieve(identifier: str) -> Optional[Dict[str, Any]]
```

Using the identifier, we search the storage and return the unique properties of the element

**Arguments**:

- `identifier`: This is the identifier that will be used to retrieve the element from the storage. See
the docs for more info.

**Returns**:

A dictionary of the unique properties

<a id="scrapling.core.storage.SQLiteStorageSystem.close"></a>

#### close

```python
def close()
```

Close all connections. It will be useful when with some things like scrapy Spider.closed() function/signal

<a id="scrapling.core.storage.SQLiteStorageSystem.__del__"></a>

#### \_\_del\_\_

```python
def __del__()
```

To ensure all connections are closed when the object is destroyed.

<a id="scrapling.core._types"></a>

# scrapling.core.\_types

Type definitions for type checking purposes.

<a id="scrapling.core.translator"></a>

# scrapling.core.translator

Most of this file is an adapted version of the parsel library's translator with some modifications simply for 1 important reason...

To add pseudo-elements ``::text`` and ``::attr(ATTR_NAME)`` so we match the Parsel/Scrapy selectors format which will be important in future releases but most importantly...

So you don't have to learn a new selectors/api method like what bs4 done with soupsieve :)

    If you want to learn about this, head to https://cssselect.readthedocs.io/en/latest/`cssselect.FunctionalPseudoElement`

<a id="scrapling.core.translator.TranslatorMixin"></a>

## TranslatorMixin Objects

```python
class TranslatorMixin()
```

This mixin adds support to CSS pseudo elements via dynamic dispatch.

Currently supported pseudo-elements are ``::text`` and ``::attr(ATTR_NAME)``.

<a id="scrapling.core.translator.TranslatorMixin.xpath_pseudo_element"></a>

#### xpath\_pseudo\_element

```python
def xpath_pseudo_element(xpath: OriginalXPathExpr,
                         pseudo_element: PseudoElement) -> OriginalXPathExpr
```

Dispatch method that transforms XPath to support the pseudo-element.

<a id="scrapling.core.translator.TranslatorMixin.xpath_attr_functional_pseudo_element"></a>

#### xpath\_attr\_functional\_pseudo\_element

```python
@staticmethod
def xpath_attr_functional_pseudo_element(
        xpath: OriginalXPathExpr,
        function: FunctionalPseudoElement) -> XPathExpr
```

Support selecting attribute values using ::attr() pseudo-element

<a id="scrapling.core.translator.TranslatorMixin.xpath_text_simple_pseudo_element"></a>

#### xpath\_text\_simple\_pseudo\_element

```python
@staticmethod
def xpath_text_simple_pseudo_element(xpath: OriginalXPathExpr) -> XPathExpr
```

Support selecting text nodes using ::text pseudo-element

<a id="scrapling.core.translator.css_to_xpath"></a>

#### css\_to\_xpath

```python
@lru_cache(maxsize=256)
def css_to_xpath(query: str) -> str
```

Return the translated XPath version of a given CSS query

<a id="scrapling.core.mixins"></a>

# scrapling.core.mixins

<a id="scrapling.core.mixins.SelectorsGeneration"></a>

## SelectorsGeneration Objects

```python
class SelectorsGeneration()
```

Functions for generating selectors
Trying to generate selectors like Firefox or maybe cleaner ones!? Ehm
Inspiration: https://searchfox.org/mozilla-central/source/devtools/shared/inspector/css-logic.js#591

<a id="scrapling.core.mixins.SelectorsGeneration.generate_css_selector"></a>

#### generate\_css\_selector

```python
@property
def generate_css_selector() -> str
```

Generate a CSS selector for the current element

**Returns**:

A string of the generated selector.

<a id="scrapling.core.mixins.SelectorsGeneration.generate_full_css_selector"></a>

#### generate\_full\_css\_selector

```python
@property
def generate_full_css_selector() -> str
```

Generate a complete CSS selector for the current element

**Returns**:

A string of the generated selector.

<a id="scrapling.core.mixins.SelectorsGeneration.generate_xpath_selector"></a>

#### generate\_xpath\_selector

```python
@property
def generate_xpath_selector() -> str
```

Generate an XPath selector for the current element

**Returns**:

A string of the generated selector.

<a id="scrapling.core.mixins.SelectorsGeneration.generate_full_xpath_selector"></a>

#### generate\_full\_xpath\_selector

```python
@property
def generate_full_xpath_selector() -> str
```

Generate a complete XPath selector for the current element

**Returns**:

A string of the generated selector.

<a id="scrapling.core"></a>

# scrapling.core

<a id="scrapling.core._shell_signatures"></a>

# scrapling.core.\_shell\_signatures

<a id="scrapling.parser"></a>

# scrapling.parser

<a id="scrapling.parser.Selector"></a>

## Selector Objects

```python
class Selector(SelectorsGeneration)
```

<a id="scrapling.parser.Selector.__init__"></a>

#### \_\_init\_\_

```python
def __init__(content: Optional[str | bytes] = None,
             url: str = "",
             encoding: str = "utf-8",
             huge_tree: bool = True,
             root: Optional[HtmlElement] = None,
             keep_comments: Optional[bool] = False,
             keep_cdata: Optional[bool] = False,
             adaptive: Optional[bool] = False,
             _storage: Optional[StorageSystemMixin] = None,
             storage: Any = SQLiteStorageSystem,
             storage_args: Optional[Dict] = None,
             **_)
```

The main class that works as a wrapper for the HTML input data. Using this class, you can search for elements

with expressions in CSS, XPath, or with simply text. Check the docs for more info.

Here we try to extend module ``lxml.html.HtmlElement`` while maintaining a simpler interface, We are not
inheriting from the ``lxml.html.HtmlElement`` because it's not pickleable, which makes a lot of reference jobs
not possible. You can test it here and see code explodes with `AssertionError: invalid Element proxy at...`.
It's an old issue with lxml, see `this entry <https://bugs.launchpad.net/lxml/+bug/736708>`

**Arguments**:

- `content`: HTML content as either string or bytes.
- `url`: It allows storing a URL with the HTML data for retrieving later.
- `encoding`: The encoding type that will be used in HTML parsing, default is `UTF-8`
- `huge_tree`: Enabled by default, should always be enabled when parsing large HTML documents. This controls
the libxml2 feature that forbids parsing certain large documents to protect from possible memory exhaustion.
- `root`: Used internally to pass etree objects instead of text/body arguments, it takes the highest priority.
Don't use it unless you know what you are doing!
- `keep_comments`: While parsing the HTML body, drop comments or not. Disabled by default for obvious reasons
- `keep_cdata`: While parsing the HTML body, drop cdata or not. Disabled by default for cleaner HTML.
- `adaptive`: Globally turn off the adaptive feature in all functions, this argument takes higher
priority over all adaptive related arguments/functions in the class.
- `storage`: The storage class to be passed for adaptive functionalities, see ``Docs`` for more info.
- `storage_args`: A dictionary of ``argument->value`` pairs to be passed for the storage class.
If empty, default values will be used.

<a id="scrapling.parser.Selector.tag"></a>

#### tag

```python
@property
def tag() -> str
```

Get the tag name of the element

<a id="scrapling.parser.Selector.text"></a>

#### text

```python
@property
def text() -> TextHandler
```

Get text content of the element

<a id="scrapling.parser.Selector.get_all_text"></a>

#### get\_all\_text

```python
def get_all_text(separator: str = "\n",
                 strip: bool = False,
                 ignore_tags: Tuple = (
                     "script",
                     "style",
                 ),
                 valid_values: bool = True) -> TextHandler
```

Get all child strings of this element, concatenated using the given separator.

**Arguments**:

- `separator`: Strings will be concatenated using this separator.
- `strip`: If True, strings will be stripped before being concatenated.
- `ignore_tags`: A tuple of all tag names you want to ignore
- `valid_values`: If enabled, elements with text-content that is empty or only whitespaces will be ignored

**Returns**:

A TextHandler

<a id="scrapling.parser.Selector.urljoin"></a>

#### urljoin

```python
def urljoin(relative_url: str) -> str
```

Join this Selector's url with a relative url to form an absolute full URL.

<a id="scrapling.parser.Selector.attrib"></a>

#### attrib

```python
@property
def attrib() -> AttributesHandler
```

Get attributes of the element

<a id="scrapling.parser.Selector.html_content"></a>

#### html\_content

```python
@property
def html_content() -> TextHandler
```

Return the inner HTML code of the element

<a id="scrapling.parser.Selector.body"></a>

#### body

```python
@property
def body() -> str | bytes
```

Return the raw body of the current `Selector` without any processing. Useful for binary and non-HTML requests.

<a id="scrapling.parser.Selector.prettify"></a>

#### prettify

```python
def prettify() -> TextHandler
```

Return a prettified version of the element's inner html-code

<a id="scrapling.parser.Selector.has_class"></a>

#### has\_class

```python
def has_class(class_name: str) -> bool
```

Check if the element has a specific class

**Arguments**:

- `class_name`: The class name to check for

**Returns**:

True if element has class with that name otherwise False

<a id="scrapling.parser.Selector.parent"></a>

#### parent

```python
@property
def parent() -> Optional["Selector"]
```

Return the direct parent of the element or ``None`` otherwise

<a id="scrapling.parser.Selector.below_elements"></a>

#### below\_elements

```python
@property
def below_elements() -> "Selectors"
```

Return all elements under the current element in the DOM tree

<a id="scrapling.parser.Selector.children"></a>

#### children

```python
@property
def children() -> "Selectors"
```

Return the children elements of the current element or empty list otherwise

<a id="scrapling.parser.Selector.siblings"></a>

#### siblings

```python
@property
def siblings() -> "Selectors"
```

Return other children of the current element's parent or empty list otherwise

<a id="scrapling.parser.Selector.iterancestors"></a>

#### iterancestors

```python
def iterancestors() -> Generator["Selector", None, None]
```

Return a generator that loops over all ancestors of the element, starting with the element's parent.

<a id="scrapling.parser.Selector.find_ancestor"></a>

#### find\_ancestor

```python
def find_ancestor(func: Callable[["Selector"], bool]) -> Optional["Selector"]
```

Loop over all ancestors of the element till one match the passed function

**Arguments**:

- `func`: A function that takes each ancestor as an argument and returns True/False

**Returns**:

The first ancestor that match the function or ``None`` otherwise.

<a id="scrapling.parser.Selector.path"></a>

#### path

```python
@property
def path() -> "Selectors"
```

Returns a list of type `Selectors` that contains the path leading to the current element from the root.

<a id="scrapling.parser.Selector.next"></a>

#### next

```python
@property
def next() -> Optional["Selector"]
```

Returns the next element of the current element in the children of the parent or ``None`` otherwise.

<a id="scrapling.parser.Selector.previous"></a>

#### previous

```python
@property
def previous() -> Optional["Selector"]
```

Returns the previous element of the current element in the children of the parent or ``None`` otherwise.

<a id="scrapling.parser.Selector.get"></a>

#### get

```python
def get() -> TextHandler
```

Serialize this element to a string.
For text nodes, returns the text value. For HTML elements, returns the outer HTML.

<a id="scrapling.parser.Selector.getall"></a>

#### getall

```python
def getall() -> TextHandlers
```

Return a single-element list containing this element's serialized string.

<a id="scrapling.parser.Selector.relocate"></a>

#### relocate

```python
def relocate(
        element: Union[Dict, HtmlElement, "Selector"],
        percentage: int = 40,
        selector_type: bool = False) -> Union[List[HtmlElement], "Selectors"]
```

This function will search again for the element in the page tree, used automatically on page structure change

**Arguments**:

- `element`: The element we want to relocate in the tree
- `percentage`: The minimum percentage to accept and not going lower than that. Be aware that the percentage
calculation depends solely on the page structure, so don't play with this number unless you must know
what you are doing!
- `selector_type`: If True, the return result will be converted to `Selectors` object

**Returns**:

List of pure HTML elements that got the highest matching score or 'Selectors' object

<a id="scrapling.parser.Selector.css"></a>

#### css

```python
def css(selector: str,
        identifier: str = "",
        adaptive: bool = False,
        auto_save: bool = False,
        percentage: int = 40) -> "Selectors"
```

Search the current tree with CSS3 selectors

**Important:
It's recommended to use the identifier argument if you plan to use a different selector later
and want to relocate the same element(s)**

**Arguments**:

- `selector`: The CSS3 selector to be used.
- `adaptive`: Enabled will make the function try to relocate the element if it was 'saved' before
- `identifier`: A string that will be used to save/retrieve element's data in adaptive,
otherwise the selector will be used.
- `auto_save`: Automatically save new elements for `adaptive` later
- `percentage`: The minimum percentage to accept while `adaptive` is working and not going lower than that.
Be aware that the percentage calculation depends solely on the page structure, so don't play with this
number unless you must know what you are doing!

**Returns**:

`Selectors` class.

<a id="scrapling.parser.Selector.xpath"></a>

#### xpath

```python
def xpath(selector: str,
          identifier: str = "",
          adaptive: bool = False,
          auto_save: bool = False,
          percentage: int = 40,
          **kwargs: Any) -> "Selectors"
```

Search the current tree with XPath selectors

**Important:
It's recommended to use the identifier argument if you plan to use a different selector later
and want to relocate the same element(s)**

 Note: **Additional keyword arguments will be passed as XPath variables in the XPath expression!**

**Arguments**:

- `selector`: The XPath selector to be used.
- `adaptive`: Enabled will make the function try to relocate the element if it was 'saved' before
- `identifier`: A string that will be used to save/retrieve element's data in adaptive,
otherwise the selector will be used.
- `auto_save`: Automatically save new elements for `adaptive` later
- `percentage`: The minimum percentage to accept while `adaptive` is working and not going lower than that.
Be aware that the percentage calculation depends solely on the page structure, so don't play with this
number unless you must know what you are doing!

**Returns**:

`Selectors` class.

<a id="scrapling.parser.Selector.find_all"></a>

#### find\_all

```python
def find_all(*args: str | Iterable[str] | Pattern | Callable | Dict[str, str],
             **kwargs: str) -> "Selectors"
```

Find elements by filters of your creations for ease.

**Arguments**:

- `args`: Tag name(s), iterable of tag names, regex patterns, function, or a dictionary of elements' attributes. Leave empty for selecting all.
- `kwargs`: The attributes you want to filter elements based on it.

**Returns**:

The `Selectors` object of the elements or empty list

<a id="scrapling.parser.Selector.find"></a>

#### find

```python
def find(*args: str | Iterable[str] | Pattern | Callable | Dict[str, str],
         **kwargs: str) -> Optional["Selector"]
```

Find elements by filters of your creations for ease, then return the first result. Otherwise return `None`.

**Arguments**:

- `args`: Tag name(s), iterable of tag names, regex patterns, function, or a dictionary of elements' attributes. Leave empty for selecting all.
- `kwargs`: The attributes you want to filter elements based on it.

**Returns**:

The `Selector` object of the element or `None` if the result didn't match

<a id="scrapling.parser.Selector.save"></a>

#### save

```python
def save(element: HtmlElement, identifier: str) -> None
```

Saves the element's unique properties to the storage for retrieval and relocation later

**Arguments**:

- `element`: The element itself that we want to save to storage, it can be a ` Selector ` or pure ` HtmlElement `
- `identifier`: This is the identifier that will be used to retrieve the element later from the storage. See
the docs for more info.

<a id="scrapling.parser.Selector.retrieve"></a>

#### retrieve

```python
def retrieve(identifier: str) -> Optional[Dict[str, Any]]
```

Using the identifier, we search the storage and return the unique properties of the element

**Arguments**:

- `identifier`: This is the identifier that will be used to retrieve the element from the storage. See
the docs for more info.

**Returns**:

A dictionary of the unique properties

<a id="scrapling.parser.Selector.json"></a>

#### json

```python
def json() -> Dict
```

Return JSON response if the response is jsonable otherwise throws error

<a id="scrapling.parser.Selector.re"></a>

#### re

```python
def re(regex: str | Pattern[str],
       replace_entities: bool = True,
       clean_match: bool = False,
       case_sensitive: bool = True) -> TextHandlers
```

Apply the given regex to the current text and return a list of strings with the matches.

**Arguments**:

- `regex`: Can be either a compiled regular expression or a string.
- `replace_entities`: If enabled character entity references are replaced by their corresponding character
- `clean_match`: if enabled, this will ignore all whitespaces and consecutive spaces while matching
- `case_sensitive`: if disabled, the function will set the regex to ignore the letters case while compiling it

<a id="scrapling.parser.Selector.re_first"></a>

#### re\_first

```python
def re_first(regex: str | Pattern[str],
             default=None,
             replace_entities: bool = True,
             clean_match: bool = False,
             case_sensitive: bool = True) -> TextHandler
```

Apply the given regex to text and return the first match if found, otherwise return the default value.

**Arguments**:

- `regex`: Can be either a compiled regular expression or a string.
- `default`: The default value to be returned if there is no match
- `replace_entities`: if enabled character entity references are replaced by their corresponding character
- `clean_match`: if enabled, this will ignore all whitespaces and consecutive spaces while matching
- `case_sensitive`: if disabled, the function will set the regex to ignore the letters case while compiling it

<a id="scrapling.parser.Selector.find_similar"></a>

#### find\_similar

```python
def find_similar(similarity_threshold: float = 0.2,
                 ignore_attributes: List | Tuple = (
                     "href",
                     "src",
                 ),
                 match_text: bool = False) -> "Selectors"
```

Find elements that are in the same tree depth in the page with the same tag name and same parent tag etc...

then return the ones that match the current element attributes with a percentage higher than the input threshold.

This function is inspired by AutoScraper and made for cases where you, for example, found a product div inside
a products-list container and want to find other products using that element as a starting point EXCEPT
this function works in any case without depending on the element type.

**Arguments**:

- `similarity_threshold`: The percentage to use while comparing element attributes.
Note: Elements found before attributes matching/comparison will be sharing the same depth, same tag name,
same parent tag name, and same grand parent tag name. So they are 99% likely to be correct unless you are
extremely unlucky, then attributes matching comes into play, so don't play with this number unless
you are getting the results you don't want.
Also, if the current element doesn't have attributes and the similar element as well, then it's a 100% match.
- `ignore_attributes`: Attribute names passed will be ignored while matching the attributes in the last step.
The default value is to ignore `href` and `src` as URLs can change a lot between elements, so it's unreliable
- `match_text`: If True, element text content will be taken into calculation while matching.
Not recommended to use in normal cases, but it depends.

**Returns**:

A ``Selectors`` container of ``Selector`` objects or empty list

<a id="scrapling.parser.Selector.find_by_text"></a>

#### find\_by\_text

```python
def find_by_text(text: str,
                 first_match: bool = True,
                 partial: bool = False,
                 case_sensitive: bool = False,
                 clean_match: bool = True) -> Union["Selectors", "Selector"]
```

Find elements that its text content fully/partially matches input.

**Arguments**:

- `text`: Text query to match
- `first_match`: Returns the first element that matches conditions, enabled by default
- `partial`: If enabled, the function returns elements that contain the input text
- `case_sensitive`: if enabled, the letters case will be taken into consideration
- `clean_match`: if enabled, this will ignore all whitespaces and consecutive spaces while matching

<a id="scrapling.parser.Selector.find_by_regex"></a>

#### find\_by\_regex

```python
def find_by_regex(query: str | Pattern[str],
                  first_match: bool = True,
                  case_sensitive: bool = False,
                  clean_match: bool = True) -> Union["Selectors", "Selector"]
```

Find elements that its text content matches the input regex pattern.

**Arguments**:

- `query`: Regex query/pattern to match
- `first_match`: Return the first element that matches conditions; enabled by default.
- `case_sensitive`: If enabled, the letters case will be taken into consideration in the regex.
- `clean_match`: If enabled, this will ignore all whitespaces and consecutive spaces while matching.

<a id="scrapling.parser.Selectors"></a>

## Selectors Objects

```python
class Selectors(List[Selector])
```

The `Selectors` class is a subclass of the builtin ``List`` class, which provides a few additional methods.

<a id="scrapling.parser.Selectors.xpath"></a>

#### xpath

```python
def xpath(selector: str,
          identifier: str = "",
          auto_save: bool = False,
          percentage: int = 40,
          **kwargs: Any) -> "Selectors"
```

Call the ``.xpath()`` method for each element in this list and return

their results as another `Selectors` class.

**Important:
It's recommended to use the identifier argument if you plan to use a different selector later
and want to relocate the same element(s)**

 Note: **Additional keyword arguments will be passed as XPath variables in the XPath expression!**

**Arguments**:

- `selector`: The XPath selector to be used.
- `identifier`: A string that will be used to retrieve element's data in adaptive,
otherwise the selector will be used.
- `auto_save`: Automatically save new elements for `adaptive` later
- `percentage`: The minimum percentage to accept while `adaptive` is working and not going lower than that.
Be aware that the percentage calculation depends solely on the page structure, so don't play with this
number unless you must know what you are doing!

**Returns**:

`Selectors` class.

<a id="scrapling.parser.Selectors.css"></a>

#### css

```python
def css(selector: str,
        identifier: str = "",
        auto_save: bool = False,
        percentage: int = 40) -> "Selectors"
```

Call the ``.css()`` method for each element in this list and return

their results flattened as another `Selectors` class.

**Important:
It's recommended to use the identifier argument if you plan to use a different selector later
and want to relocate the same element(s)**

**Arguments**:

- `selector`: The CSS3 selector to be used.
- `identifier`: A string that will be used to retrieve element's data in adaptive,
otherwise the selector will be used.
- `auto_save`: Automatically save new elements for `adaptive` later
- `percentage`: The minimum percentage to accept while `adaptive` is working and not going lower than that.
Be aware that the percentage calculation depends solely on the page structure, so don't play with this
number unless you must know what you are doing!

**Returns**:

`Selectors` class.

<a id="scrapling.parser.Selectors.re"></a>

#### re

```python
def re(regex: str | Pattern,
       replace_entities: bool = True,
       clean_match: bool = False,
       case_sensitive: bool = True) -> TextHandlers
```

Call the ``.re()`` method for each element in this list and return

their results flattened as List of TextHandler.

**Arguments**:

- `regex`: Can be either a compiled regular expression or a string.
- `replace_entities`: If enabled character entity references are replaced by their corresponding character
- `clean_match`: if enabled, this will ignore all whitespaces and consecutive spaces while matching
- `case_sensitive`: if disabled, the function will set the regex to ignore the letters case while compiling it

<a id="scrapling.parser.Selectors.re_first"></a>

#### re\_first

```python
def re_first(regex: str | Pattern,
             default: Any = None,
             replace_entities: bool = True,
             clean_match: bool = False,
             case_sensitive: bool = True) -> TextHandler
```

Call the ``.re_first()`` method for each element in this list and return

the first result or the default value otherwise.

**Arguments**:

- `regex`: Can be either a compiled regular expression or a string.
- `default`: The default value to be returned if there is no match
- `replace_entities`: if enabled character entity references are replaced by their corresponding character
- `clean_match`: if enabled, this will ignore all whitespaces and consecutive spaces while matching
- `case_sensitive`: if disabled, function will set the regex to ignore the letters case while compiling it

<a id="scrapling.parser.Selectors.search"></a>

#### search

```python
def search(func: Callable[["Selector"], bool]) -> Optional["Selector"]
```

Loop over all current elements and return the first element that matches the passed function

**Arguments**:

- `func`: A function that takes each element as an argument and returns True/False

**Returns**:

The first element that match the function or ``None`` otherwise.

<a id="scrapling.parser.Selectors.filter"></a>

#### filter

```python
def filter(func: Callable[["Selector"], bool]) -> "Selectors"
```

Filter current elements based on the passed function

**Arguments**:

- `func`: A function that takes each element as an argument and returns True/False

**Returns**:

The new `Selectors` object or empty list otherwise.

<a id="scrapling.parser.Selectors.get"></a>

#### get

```python
def get(default=None)
```

Returns the serialized string of the first element, or ``default`` if empty.

**Arguments**:

- `default`: the default value to return if the current list is empty

<a id="scrapling.parser.Selectors.getall"></a>

#### getall

```python
def getall() -> TextHandlers
```

Serialize all elements and return as a TextHandlers list.

<a id="scrapling.parser.Selectors.first"></a>

#### first

```python
@property
def first() -> Optional[Selector]
```

Returns the first Selector item of the current list or `None` if the list is empty

<a id="scrapling.parser.Selectors.last"></a>

#### last

```python
@property
def last() -> Optional[Selector]
```

Returns the last Selector item of the current list or `None` if the list is empty

<a id="scrapling.parser.Selectors.length"></a>

#### length

```python
@property
def length() -> int
```

Returns the length of the current list

<a id="scrapling.engines.toolbelt.proxy_rotation"></a>

# scrapling.engines.toolbelt.proxy\_rotation

<a id="scrapling.engines.toolbelt.proxy_rotation.is_proxy_error"></a>

#### is\_proxy\_error

```python
def is_proxy_error(error: Exception) -> bool
```

Check if an error is proxy-related. Works for both HTTP and browser errors.

<a id="scrapling.engines.toolbelt.proxy_rotation.cyclic_rotation"></a>

#### cyclic\_rotation

```python
def cyclic_rotation(proxies: List[ProxyType],
                    current_index: int) -> Tuple[ProxyType, int]
```

Default cyclic rotation strategy - iterates through proxies sequentially, wrapping around at the end.

<a id="scrapling.engines.toolbelt.proxy_rotation.ProxyRotator"></a>

## ProxyRotator Objects

```python
class ProxyRotator()
```

A thread-safe proxy rotator with pluggable rotation strategies.

Supports:
- Cyclic rotation (default)
- Custom rotation strategies via callable
- Both string URLs and Playwright-style dict proxies

<a id="scrapling.engines.toolbelt.proxy_rotation.ProxyRotator.__init__"></a>

#### \_\_init\_\_

```python
def __init__(proxies: List[ProxyType],
             strategy: RotationStrategy = cyclic_rotation)
```

Initialize the proxy rotator.

**Arguments**:

- `proxies`: List of proxy URLs or Playwright-style proxy dicts.
- String format: "http://proxy1:8080" or "http://user:pass@proxy:8080"
- Dict format: {"server": "http://proxy:8080", "username": "user", "password": "pass"}
- `strategy`: Rotation strategy function. Takes (proxies, current_index) and returns (proxy, next_index). Defaults to cyclic_rotation.

<a id="scrapling.engines.toolbelt.proxy_rotation.ProxyRotator.get_proxy"></a>

#### get\_proxy

```python
def get_proxy() -> ProxyType
```

Get the next proxy according to the rotation strategy.

<a id="scrapling.engines.toolbelt.proxy_rotation.ProxyRotator.proxies"></a>

#### proxies

```python
@property
def proxies() -> List[ProxyType]
```

Get a copy of all configured proxies.

<a id="scrapling.engines.toolbelt.proxy_rotation.ProxyRotator.__len__"></a>

#### \_\_len\_\_

```python
def __len__() -> int
```

Return the total number of configured proxies.

<a id="scrapling.engines.toolbelt.fingerprints"></a>

# scrapling.engines.toolbelt.fingerprints

Functions related to generating headers and fingerprints generally

<a id="scrapling.engines.toolbelt.fingerprints.get_os_name"></a>

#### get\_os\_name

```python
@lru_cache(1, typed=True)
def get_os_name() -> OSName | Tuple
```

Get the current OS name in the same format needed for browserforge, if the OS is Unknown, return None so browserforge uses all.

**Returns**:

Current OS name or `None` otherwise

<a id="scrapling.engines.toolbelt.fingerprints.generate_headers"></a>

#### generate\_headers

```python
def generate_headers(browser_mode: bool | str = False) -> Dict
```

Generate real browser-like headers using browserforge's generator

**Arguments**:

- `browser_mode`: If enabled, the headers created are used for playwright, so it has to match everything

**Returns**:

A dictionary of the generated headers

<a id="scrapling.engines.toolbelt.convertor"></a>

# scrapling.engines.toolbelt.convertor

<a id="scrapling.engines.toolbelt.convertor.ResponseFactory"></a>

## ResponseFactory Objects

```python
class ResponseFactory()
```

Factory class for creating `Response` objects from various sources.

This class provides multiple static and instance methods for building standardized `Response` objects
from diverse input sources such as Playwright responses, asynchronous Playwright responses,
and raw HTTP request responses. It supports handling response histories, constructing the proper
response objects, and managing encoding, headers, cookies, and other attributes.

<a id="scrapling.engines.toolbelt.convertor.ResponseFactory.from_playwright_response"></a>

#### from\_playwright\_response

```python
@classmethod
def from_playwright_response(cls,
                             page: Optional[SyncPage],
                             first_response: SyncResponse,
                             final_response: Optional[SyncResponse],
                             parser_arguments: Dict,
                             meta: Optional[Dict] = None,
                             xhr_captured: Optional[List[SyncResponse]] = None,
                             collect_history: bool = True) -> Response
```

Transforms a Playwright response into an internal `Response` object, encapsulating

the page's content, response status, headers, and relevant metadata.

The function handles potential issues, such as empty or missing final responses,
by falling back to the first response if necessary. Encoding and status text
are also derived from the provided response headers or reasonable defaults.
Additionally, the page content and cookies are extracted for further use.

**Arguments**:

- `page`: A synchronous Playwright `Page` instance that represents the current browser page. Required to retrieve the page's URL, cookies, and content.
- `final_response`: The last response received for the given request from the Playwright instance. Typically used as the main response object to derive status, headers, and other metadata.
- `first_response`: An earlier or initial Playwright `Response` object that may serve as a fallback response in the absence of the final one.
- `parser_arguments`: A dictionary containing additional arguments needed for parsing or further customization of the returned `Response`. These arguments are dynamically unpacked into
the `Response` object.
- `meta`: Additional meta data to be saved with the response.
- `xhr_captured`: Optional list of captured Playwright XHR/fetch responses to convert and attach to the returned Response.
- `collect_history`: Optional boolean indicating whether to collect redirections history or not.

**Returns**:

`Response`: A fully populated `Response` object containing the page's URL, content, status, headers, cookies, and other derived metadata.

<a id="scrapling.engines.toolbelt.convertor.ResponseFactory.from_async_playwright_response"></a>

#### from\_async\_playwright\_response

```python
@classmethod
async def from_async_playwright_response(
        cls,
        page: Optional[AsyncPage],
        first_response: AsyncResponse,
        final_response: Optional[AsyncResponse],
        parser_arguments: Dict,
        meta: Optional[Dict] = None,
        xhr_captured: Optional[List[AsyncResponse]] = None,
        collect_history: bool = True) -> Response
```

Transforms a Playwright response into an internal `Response` object, encapsulating

the page's content, response status, headers, and relevant metadata.

The function handles potential issues, such as empty or missing final responses,
by falling back to the first response if necessary. Encoding and status text
are also derived from the provided response headers or reasonable defaults.
Additionally, the page content and cookies are extracted for further use.

**Arguments**:

- `page`: An asynchronous Playwright `Page` instance that represents the current browser page. Required to retrieve the page's URL, cookies, and content.
- `final_response`: The last response received for the given request from the Playwright instance. Typically used as the main response object to derive status, headers, and other metadata.
- `first_response`: An earlier or initial Playwright `Response` object that may serve as a fallback response in the absence of the final one.
- `parser_arguments`: A dictionary containing additional arguments needed for parsing or further customization of the returned `Response`. These arguments are dynamically unpacked into
the `Response` object.
- `meta`: Additional meta data to be saved with the response.
- `xhr_captured`: Optional list of captured async Playwright XHR/fetch responses to convert and attach to the returned Response.
- `collect_history`: Optional boolean indicating whether to collect redirections history or not.

**Returns**:

`Response`: A fully populated `Response` object containing the page's URL, content, status, headers, cookies, and other derived metadata.

<a id="scrapling.engines.toolbelt.convertor.ResponseFactory.from_http_request"></a>

#### from\_http\_request

```python
@staticmethod
def from_http_request(response: CurlResponse,
                      parser_arguments: Dict,
                      meta: Optional[Dict] = None) -> Response
```

Takes `curl_cffi` response and generates `Response` object from it.

**Arguments**:

- `response`: `curl_cffi` response object
- `parser_arguments`: Additional arguments to be passed to the `Response` object constructor.
- `meta`: Optional metadata dictionary to attach to the Response.

**Returns**:

A `Response` object that is the same as `Selector` object except it has these added attributes: `status`, `reason`, `cookies`, `headers`, and `request_headers`

<a id="scrapling.engines.toolbelt.ad_domains"></a>

# scrapling.engines.toolbelt.ad\_domains

Built-in ad/tracker domain list for use with block_ads=True.

Source: Peter Lowe's ad and tracking server list https://pgl.yoyo.org/adservers/
Used config: https://pgl.yoyo.org/adservers/serverlist.php?hostformat=plain&showintro=0&startyear=2000&mimetype=plaintext

<a id="scrapling.engines.toolbelt.custom"></a>

# scrapling.engines.toolbelt.custom

Functions related to custom types or type checking

<a id="scrapling.engines.toolbelt.custom.Response"></a>

## Response Objects

```python
class Response(Selector)
```

This class is returned by all engines as a way to unify the response type between different libraries.

**Arguments**:

- `status`: HTTP status code.
- `reason`: HTTP status message.
- `cookies`: Response cookies.
- `headers`: Response headers.
- `request_headers`: Request headers sent with the request.
- `history`: List of redirect responses, if any.
- `meta`: Metadata dictionary (e.g., proxy used).
- `request`: Associated spider Request object (set by crawler, in the spiders framework).
- `captured_xhr`: List of captured XHR/fetch ``Response`` objects. Populated when ``capture_xhr`` is set on a browser session.

<a id="scrapling.engines.toolbelt.custom.Response.body"></a>

#### body

```python
@property
def body() -> bytes
```

Return the raw body of the response as bytes.

<a id="scrapling.engines.toolbelt.custom.Response.follow"></a>

#### follow

```python
def follow(url: str,
           sid: str = "",
           callback: Callable[["Response"],
                              AsyncGenerator[Union[Dict[str, Any], "Request",
                                                   None], None]] | None = None,
           priority: int | None = None,
           dont_filter: bool = False,
           meta: dict[str, Any] | None = None,
           referer_flow: bool = True,
           **kwargs: Any) -> Any
```

Create a Request to follow a URL.

This is a helper method for spiders to easily follow links found in pages.

**IMPORTANT**: The below arguments if left empty, the corresponding value from the previous request will be used. The only exception is `dont_filter`.

**Arguments**:

- `url`: The URL to follow (can be relative, will be joined with current URL)
- `sid`: The session id to use
- `callback`: Spider callback method to use
- `priority`: The priority number to use, the higher the number, the higher priority to be processed first.
- `dont_filter`: If this request has been done before, disable the filter to allow it again.
- `meta`: Additional meta data to included in the request
- `referer_flow`: Enabled by default, set the current response url as referer for the new request url.
- `kwargs`: Additional Request arguments

**Returns**:

Request object ready to be yielded

<a id="scrapling.engines.toolbelt.custom.BaseFetcher"></a>

## BaseFetcher Objects

```python
class BaseFetcher()
```

<a id="scrapling.engines.toolbelt.custom.BaseFetcher.parser_keywords"></a>

#### parser\_keywords

Left open for the user

<a id="scrapling.engines.toolbelt.custom.BaseFetcher.configure"></a>

#### configure

```python
@classmethod
def configure(cls, **kwargs)
```

Set multiple arguments for the parser at once globally

**Arguments**:

- `kwargs`: The keywords can be any arguments of the following: huge_tree, keep_comments, keep_cdata, adaptive, storage, storage_args, adaptive_domain

<a id="scrapling.engines.toolbelt.custom.StatusText"></a>

## StatusText Objects

```python
class StatusText()
```

A class that gets the status text of the response status code.

Reference: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

<a id="scrapling.engines.toolbelt.custom.StatusText.get"></a>

#### get

```python
@classmethod
@lru_cache(maxsize=128)
def get(cls, status_code: int) -> str
```

Get the phrase for a given HTTP status code.

<a id="scrapling.engines.toolbelt.navigation"></a>

# scrapling.engines.toolbelt.navigation

Functions related to files and URLs

<a id="scrapling.engines.toolbelt.navigation.create_intercept_handler"></a>

#### create\_intercept\_handler

```python
def create_intercept_handler(
        disable_resources: bool,
        blocked_domains: Optional[Set[str]] = None) -> Callable
```

Create a route handler that blocks both resource types and specific domains.

**Arguments**:

- `disable_resources`: Whether to block default resource types.
- `blocked_domains`: Set of domain names to block requests to.

**Returns**:

A sync route handler function.

<a id="scrapling.engines.toolbelt.navigation.create_async_intercept_handler"></a>

#### create\_async\_intercept\_handler

```python
def create_async_intercept_handler(
        disable_resources: bool,
        blocked_domains: Optional[Set[str]] = None) -> Callable
```

Create an async route handler that blocks both resource types and specific domains.

**Arguments**:

- `disable_resources`: Whether to block default resource types.
- `blocked_domains`: Set of domain names to block requests to.

**Returns**:

An async route handler function.

<a id="scrapling.engines.toolbelt.navigation.construct_proxy_dict"></a>

#### construct\_proxy\_dict

```python
def construct_proxy_dict(proxy_string: str | Dict[str, str] | Tuple) -> Dict
```

Validate a proxy and return it in the acceptable format for Playwright

Reference: https://playwright.dev/python/docs/network#http-proxy

**Arguments**:

- `proxy_string`: A string or a dictionary representation of the proxy.

<a id="scrapling.engines.toolbelt"></a>

# scrapling.engines.toolbelt

<a id="scrapling.engines._browsers._controllers"></a>

# scrapling.engines.\_browsers.\_controllers

<a id="scrapling.engines._browsers._controllers.DynamicSession"></a>

## DynamicSession Objects

```python
class DynamicSession(SyncSession, DynamicSessionMixin)
```

A Browser session manager with page pooling.

<a id="scrapling.engines._browsers._controllers.DynamicSession.__init__"></a>

#### \_\_init\_\_

```python
def __init__(**kwargs: Unpack[PlaywrightSession])
```

A Browser session manager with page pooling, it's using a persistent browser Context by default with a temporary user profile directory.

**Arguments**:

- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `init_script`: An absolute path to a JavaScript file to be executed on page creation for all pages in this session.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `user_data_dir`: Path to a User Data Directory, which stores browser session data like cookies and local storage. The default is to create a temporary directory.
- `extra_flags`: A list of additional browser flags to pass to the browser on launch.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings, and it takes higher priority than Scrapling's settings.

<a id="scrapling.engines._browsers._controllers.DynamicSession.start"></a>

#### start

```python
def start()
```

Create a browser for this instance and context.

<a id="scrapling.engines._browsers._controllers.DynamicSession.fetch"></a>

#### fetch

```python
def fetch(url: str, **kwargs: Unpack[PlaywrightFetchParams]) -> Response
```

Opens up the browser and do your request based on your chosen options.

**Arguments**:

- `url`: The Target url.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `proxy`: Static proxy to override rotator and session proxy. A new browser context will be created and used with it.

**Returns**:

A `Response` object.

<a id="scrapling.engines._browsers._controllers.AsyncDynamicSession"></a>

## AsyncDynamicSession Objects

```python
class AsyncDynamicSession(AsyncSession, DynamicSessionMixin)
```

An async Browser session manager with page pooling, it's using a persistent browser Context by default with a temporary user profile directory.

<a id="scrapling.engines._browsers._controllers.AsyncDynamicSession.__init__"></a>

#### \_\_init\_\_

```python
def __init__(**kwargs: Unpack[PlaywrightSession])
```

A Browser session manager with page pooling

**Arguments**:

- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `init_script`: An absolute path to a JavaScript file to be executed on page creation for all pages in this session.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `max_pages`: The maximum number of tabs to be opened at the same time. It will be used in rotation through a PagePool.
- `user_data_dir`: Path to a User Data Directory, which stores browser session data like cookies and local storage. The default is to create a temporary directory.
- `extra_flags`: A list of additional browser flags to pass to the browser on launch.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings, and it takes higher priority than Scrapling's settings.

<a id="scrapling.engines._browsers._controllers.AsyncDynamicSession.start"></a>

#### start

```python
async def start() -> None
```

Create a browser for this instance and context.

<a id="scrapling.engines._browsers._controllers.AsyncDynamicSession.fetch"></a>

#### fetch

```python
async def fetch(url: str, **kwargs: Unpack[PlaywrightFetchParams]) -> Response
```

Opens up the browser and do your request based on your chosen options.

**Arguments**:

- `url`: The Target url.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `proxy`: Static proxy to override rotator and session proxy. A new browser context will be created and used with it.

**Returns**:

A `Response` object.

<a id="scrapling.engines._browsers._config_tools"></a>

# scrapling.engines.\_browsers.\_config\_tools

<a id="scrapling.engines._browsers._stealth"></a>

# scrapling.engines.\_browsers.\_stealth

<a id="scrapling.engines._browsers._stealth.StealthySession"></a>

## StealthySession Objects

```python
class StealthySession(SyncSession, StealthySessionMixin)
```

A Stealthy Browser session manager with page pooling.

<a id="scrapling.engines._browsers._stealth.StealthySession.__init__"></a>

#### \_\_init\_\_

```python
def __init__(**kwargs: Unpack[StealthSession])
```

A Browser session manager with page pooling, it's using a persistent browser Context by default with a temporary user profile directory.

**Arguments**:

- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `init_script`: An absolute path to a JavaScript file to be executed on page creation for all pages in this session.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `solve_cloudflare`: Solves all types of the Cloudflare's Turnstile/Interstitial challenges before returning the response to you.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `hide_canvas`: Add random noise to canvas operations to prevent fingerprinting.
- `block_webrtc`: Forces WebRTC to respect proxy settings to prevent local IP address leak.
- `allow_webgl`: Enabled by default. Disabling it disables WebGL and WebGL 2.0 support entirely. Disabling WebGL is not recommended as many WAFs now check if WebGL is enabled.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `user_data_dir`: Path to a User Data Directory, which stores browser session data like cookies and local storage. The default is to create a temporary directory.
- `extra_flags`: A list of additional browser flags to pass to the browser on launch.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings, and it takes higher priority than Scrapling's settings.

<a id="scrapling.engines._browsers._stealth.StealthySession.start"></a>

#### start

```python
def start() -> None
```

Create a browser for this instance and context.

<a id="scrapling.engines._browsers._stealth.StealthySession.fetch"></a>

#### fetch

```python
def fetch(url: str, **kwargs: Unpack[StealthFetchParams]) -> Response
```

Opens up the browser and do your request based on your chosen options.

**Arguments**:

- `url`: The Target url.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `solve_cloudflare`: Solves all types of the Cloudflare's Turnstile/Interstitial challenges before returning the response to you.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `proxy`: Static proxy to override rotator and session proxy. A new browser context will be created and used with it.

**Returns**:

A `Response` object.

<a id="scrapling.engines._browsers._stealth.AsyncStealthySession"></a>

## AsyncStealthySession Objects

```python
class AsyncStealthySession(AsyncSession, StealthySessionMixin)
```

An async Stealthy Browser session manager with page pooling.

<a id="scrapling.engines._browsers._stealth.AsyncStealthySession.__init__"></a>

#### \_\_init\_\_

```python
def __init__(**kwargs: Unpack[StealthSession])
```

A Browser session manager with page pooling, it's using a persistent browser Context by default with a temporary user profile directory.

**Arguments**:

- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `init_script`: An absolute path to a JavaScript file to be executed on page creation for all pages in this session.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `solve_cloudflare`: Solves all types of the Cloudflare's Turnstile/Interstitial challenges before returning the response to you.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `hide_canvas`: Add random noise to canvas operations to prevent fingerprinting.
- `block_webrtc`: Forces WebRTC to respect proxy settings to prevent local IP address leak.
- `allow_webgl`: Enabled by default. Disabling it disables WebGL and WebGL 2.0 support entirely. Disabling WebGL is not recommended as many WAFs now check if WebGL is enabled.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `user_data_dir`: Path to a User Data Directory, which stores browser session data like cookies and local storage. The default is to create a temporary directory.
- `extra_flags`: A list of additional browser flags to pass to the browser on launch.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings, and it takes higher priority than Scrapling's settings.

<a id="scrapling.engines._browsers._stealth.AsyncStealthySession.start"></a>

#### start

```python
async def start() -> None
```

Create a browser for this instance and context.

<a id="scrapling.engines._browsers._stealth.AsyncStealthySession.fetch"></a>

#### fetch

```python
async def fetch(url: str, **kwargs: Unpack[StealthFetchParams]) -> Response
```

Opens up the browser and do your request based on your chosen options.

**Arguments**:

- `url`: The Target url.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `solve_cloudflare`: Solves all types of the Cloudflare's Turnstile/Interstitial challenges before returning the response to you.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `proxy`: Static proxy to override rotator and session proxy. A new browser context will be created and used with it.

**Returns**:

A `Response` object.

<a id="scrapling.engines._browsers._base"></a>

# scrapling.engines.\_browsers.\_base

<a id="scrapling.engines._browsers._base.SyncSession"></a>

## SyncSession Objects

```python
class SyncSession()
```

<a id="scrapling.engines._browsers._base.SyncSession.close"></a>

#### close

```python
def close()
```

Close all resources

<a id="scrapling.engines._browsers._base.SyncSession.get_pool_stats"></a>

#### get\_pool\_stats

```python
def get_pool_stats() -> Dict[str, int]
```

Get statistics about the current page pool

<a id="scrapling.engines._browsers._base.AsyncSession"></a>

## AsyncSession Objects

```python
class AsyncSession()
```

<a id="scrapling.engines._browsers._base.AsyncSession.close"></a>

#### close

```python
async def close()
```

Close all resources

<a id="scrapling.engines._browsers._base.AsyncSession.get_pool_stats"></a>

#### get\_pool\_stats

```python
def get_pool_stats() -> Dict[str, int]
```

Get statistics about the current page pool

<a id="scrapling.engines._browsers._page"></a>

# scrapling.engines.\_browsers.\_page

<a id="scrapling.engines._browsers._page.PageState"></a>

#### PageState

States that a page can be in

<a id="scrapling.engines._browsers._page.PageInfo"></a>

## PageInfo Objects

```python
@dataclass
class PageInfo(Generic[PageType])
```

Information about the page and its current state

<a id="scrapling.engines._browsers._page.PageInfo.mark_busy"></a>

#### mark\_busy

```python
def mark_busy(url: str = "")
```

Mark the page as busy

<a id="scrapling.engines._browsers._page.PageInfo.mark_error"></a>

#### mark\_error

```python
def mark_error()
```

Mark the page as having an error

<a id="scrapling.engines._browsers._page.PageInfo.__eq__"></a>

#### \_\_eq\_\_

```python
def __eq__(other_page)
```

Comparing this page to another page object.

<a id="scrapling.engines._browsers._page.PagePool"></a>

## PagePool Objects

```python
class PagePool()
```

Manages a pool of browser pages/tabs with state tracking

<a id="scrapling.engines._browsers._page.PagePool.add_page"></a>

#### add\_page

```python
def add_page(
        page: SyncPage | AsyncPage
) -> PageInfo[SyncPage] | PageInfo[AsyncPage]
```

Add a new page to the pool

<a id="scrapling.engines._browsers._page.PagePool.pages_count"></a>

#### pages\_count

```python
@property
def pages_count() -> int
```

Get the total number of pages

<a id="scrapling.engines._browsers._page.PagePool.busy_count"></a>

#### busy\_count

```python
@property
def busy_count() -> int
```

Get the number of busy pages

<a id="scrapling.engines._browsers._page.PagePool.cleanup_error_pages"></a>

#### cleanup\_error\_pages

```python
def cleanup_error_pages()
```

Remove pages in error state

<a id="scrapling.engines._browsers._types"></a>

# scrapling.engines.\_browsers.\_types

<a id="scrapling.engines._browsers._validators"></a>

# scrapling.engines.\_browsers.\_validators

<a id="scrapling.engines._browsers._validators.PlaywrightConfig"></a>

## PlaywrightConfig Objects

```python
class PlaywrightConfig(Struct)
```

Configuration struct for validation

<a id="scrapling.engines._browsers._validators.PlaywrightConfig.proxy"></a>

#### proxy

The default value for proxy in Playwright's source is `None`

<a id="scrapling.engines._browsers._validators.PlaywrightConfig.__post_init__"></a>

#### \_\_post\_init\_\_

```python
def __post_init__()
```

Custom validation after msgspec validation

<a id="scrapling.engines._browsers._validators.StealthConfig"></a>

## StealthConfig Objects

```python
class StealthConfig(PlaywrightConfig)
```

<a id="scrapling.engines._browsers._validators.StealthConfig.__post_init__"></a>

#### \_\_post\_init\_\_

```python
def __post_init__()
```

Custom validation after msgspec validation

<a id="scrapling.engines._browsers"></a>

# scrapling.engines.\_browsers

<a id="scrapling.engines.constants"></a>

# scrapling.engines.constants

<a id="scrapling.engines.static"></a>

# scrapling.engines.static

<a id="scrapling.engines.static._SyncSessionLogic"></a>

## \_SyncSessionLogic Objects

```python
class _SyncSessionLogic(_ConfigurationLogic)
```

<a id="scrapling.engines.static._SyncSessionLogic.__enter__"></a>

#### \_\_enter\_\_

```python
def __enter__()
```

Creates and returns a new synchronous Fetcher Session

<a id="scrapling.engines.static._SyncSessionLogic.__exit__"></a>

#### \_\_exit\_\_

```python
def __exit__(exc_type, exc_val, exc_tb)
```

Closes the active synchronous session managed by this instance, if any.

<a id="scrapling.engines.static._SyncSessionLogic.get"></a>

#### get

```python
def get(url: str, **kwargs: Unpack[GetRequestParams]) -> Response
```

Perform a GET request.

Any additional keyword arguments are passed to the `curl_cffi.requests.Session().request()` method.

**Arguments**:

- `url`: Target URL for the request.
- `kwargs`: Additional keyword arguments including:
- params: Query string parameters for the request.
- headers: Headers to include in the request.
- cookies: Cookies to use in the request.
- timeout: Number of seconds to wait before timing out.
- follow_redirects: Whether to follow redirects. Defaults to "safe" (rejects redirects to internal/private IPs).
- max_redirects: Maximum number of redirects. Default 30, use -1 for unlimited.
- retries: Number of retry attempts. Defaults to 3.
- retry_delay: Number of seconds to wait between retry attempts. Defaults to 1 second.
- proxies: Dict of proxies to use.
- proxy: Proxy URL to use. Format: "http://username:password@localhost:8030".
- proxy_auth: HTTP basic auth for proxy, tuple of (username, password).
- auth: HTTP basic auth tuple of (username, password). Only basic auth is supported.
- verify: Whether to verify HTTPS certificates.
- cert: Tuple of (cert, key) filenames for the client certificate.
- impersonate: Browser version to impersonate. Automatically defaults to the latest available Chrome version.
- http3: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- stealthy_headers: If enabled (default), it creates and adds real browser headers.

**Returns**:

A `Response` object.

<a id="scrapling.engines.static._SyncSessionLogic.post"></a>

#### post

```python
def post(url: str, **kwargs: Unpack[DataRequestParams]) -> Response
```

Perform a POST request.

Any additional keyword arguments are passed to the `curl_cffi.requests.Session().request()` method.

**Arguments**:

- `url`: Target URL for the request.
- `kwargs`: Additional keyword arguments including:
- data: Form data to include in the request body.
- json: A JSON serializable object to include in the body of the request.
- params: Query string parameters for the request.
- headers: Headers to include in the request.
- cookies: Cookies to use in the request.
- timeout: Number of seconds to wait before timing out.
- follow_redirects: Whether to follow redirects. Defaults to "safe" (rejects redirects to internal/private IPs).
- max_redirects: Maximum number of redirects. Default 30, use -1 for unlimited.
- retries: Number of retry attempts. Defaults to 3.
- retry_delay: Number of seconds to wait between retry attempts. Defaults to 1 second.
- proxies: Dict of proxies to use.
- proxy: Proxy URL to use. Format: "http://username:password@localhost:8030".
- proxy_auth: HTTP basic auth for proxy, tuple of (username, password).
- auth: HTTP basic auth tuple of (username, password). Only basic auth is supported.
- verify: Whether to verify HTTPS certificates.
- cert: Tuple of (cert, key) filenames for the client certificate.
- impersonate: Browser version to impersonate. Automatically defaults to the latest available Chrome version.
- http3: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- stealthy_headers: If enabled (default), it creates and adds real browser headers.

**Returns**:

A `Response` object.

<a id="scrapling.engines.static._SyncSessionLogic.put"></a>

#### put

```python
def put(url: str, **kwargs: Unpack[DataRequestParams]) -> Response
```

Perform a PUT request.

Any additional keyword arguments are passed to the `curl_cffi.requests.Session().request()` method.

**Arguments**:

- `url`: Target URL for the request.
- `kwargs`: Additional keyword arguments including:
- data: Form data to include in the request body.
- json: A JSON serializable object to include in the body of the request.
- params: Query string parameters for the request.
- headers: Headers to include in the request.
- cookies: Cookies to use in the request.
- timeout: Number of seconds to wait before timing out.
- follow_redirects: Whether to follow redirects. Defaults to "safe" (rejects redirects to internal/private IPs).
- max_redirects: Maximum number of redirects. Default 30, use -1 for unlimited.
- retries: Number of retry attempts. Defaults to 3.
- retry_delay: Number of seconds to wait between retry attempts. Defaults to 1 second.
- proxies: Dict of proxies to use.
- proxy: Proxy URL to use. Format: "http://username:password@localhost:8030".
- proxy_auth: HTTP basic auth for proxy, tuple of (username, password).
- auth: HTTP basic auth tuple of (username, password). Only basic auth is supported.
- verify: Whether to verify HTTPS certificates.
- cert: Tuple of (cert, key) filenames for the client certificate.
- impersonate: Browser version to impersonate. Automatically defaults to the latest available Chrome version.
- http3: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- stealthy_headers: If enabled (default), it creates and adds real browser headers.

**Returns**:

A `Response` object.

<a id="scrapling.engines.static._SyncSessionLogic.delete"></a>

#### delete

```python
def delete(url: str, **kwargs: Unpack[DataRequestParams]) -> Response
```

Perform a DELETE request.

Any additional keyword arguments are passed to the `curl_cffi.requests.Session().request()` method.

**Arguments**:

- `url`: Target URL for the request.
- `kwargs`: Additional keyword arguments including:
- data: Form data to include in the request body.
- json: A JSON serializable object to include in the body of the request.
- params: Query string parameters for the request.
- headers: Headers to include in the request.
- cookies: Cookies to use in the request.
- timeout: Number of seconds to wait before timing out.
- follow_redirects: Whether to follow redirects. Defaults to "safe" (rejects redirects to internal/private IPs).
- max_redirects: Maximum number of redirects. Default 30, use -1 for unlimited.
- retries: Number of retry attempts. Defaults to 3.
- retry_delay: Number of seconds to wait between retry attempts. Defaults to 1 second.
- proxies: Dict of proxies to use.
- proxy: Proxy URL to use. Format: "http://username:password@localhost:8030".
- proxy_auth: HTTP basic auth for proxy, tuple of (username, password).
- auth: HTTP basic auth tuple of (username, password). Only basic auth is supported.
- verify: Whether to verify HTTPS certificates.
- cert: Tuple of (cert, key) filenames for the client certificate.
- impersonate: Browser version to impersonate. Automatically defaults to the latest available Chrome version.
- http3: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- stealthy_headers: If enabled (default), it creates and adds real browser headers.

**Returns**:

A `Response` object.

<a id="scrapling.engines.static._ASyncSessionLogic"></a>

## \_ASyncSessionLogic Objects

```python
class _ASyncSessionLogic(_ConfigurationLogic)
```

<a id="scrapling.engines.static._ASyncSessionLogic.__aenter__"></a>

#### \_\_aenter\_\_

```python
async def __aenter__()
```

Creates and returns a new asynchronous Session.

<a id="scrapling.engines.static._ASyncSessionLogic.__aexit__"></a>

#### \_\_aexit\_\_

```python
async def __aexit__(exc_type, exc_val, exc_tb)
```

Closes the active asynchronous session managed by this instance, if any.

<a id="scrapling.engines.static._ASyncSessionLogic.get"></a>

#### get

```python
def get(url: str, **kwargs: Unpack[GetRequestParams]) -> Awaitable[Response]
```

Perform a GET request.

Any additional keyword arguments are passed to the `curl_cffi.requests.AsyncSession().request()` method.

**Arguments**:

- `url`: Target URL for the request.
- `kwargs`: Additional keyword arguments including:
- params: Query string parameters for the request.
- headers: Headers to include in the request.
- cookies: Cookies to use in the request.
- timeout: Number of seconds to wait before timing out.
- follow_redirects: Whether to follow redirects. Defaults to "safe" (rejects redirects to internal/private IPs).
- max_redirects: Maximum number of redirects. Default 30, use -1 for unlimited.
- retries: Number of retry attempts. Defaults to 3.
- retry_delay: Number of seconds to wait between retry attempts. Defaults to 1 second.
- proxies: Dict of proxies to use.
- proxy: Proxy URL to use. Format: "http://username:password@localhost:8030".
- proxy_auth: HTTP basic auth for proxy, tuple of (username, password).
- auth: HTTP basic auth tuple of (username, password). Only basic auth is supported.
- verify: Whether to verify HTTPS certificates.
- cert: Tuple of (cert, key) filenames for the client certificate.
- impersonate: Browser version to impersonate. Automatically defaults to the latest available Chrome version.
- http3: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- stealthy_headers: If enabled (default), it creates and adds real browser headers.

**Returns**:

A `Response` object.

<a id="scrapling.engines.static._ASyncSessionLogic.post"></a>

#### post

```python
def post(url: str, **kwargs: Unpack[DataRequestParams]) -> Awaitable[Response]
```

Perform a POST request.

Any additional keyword arguments are passed to the `curl_cffi.requests.AsyncSession().request()` method.

**Arguments**:

- `url`: Target URL for the request.
- `kwargs`: Additional keyword arguments including:
- data: Form data to include in the request body.
- json: A JSON serializable object to include in the body of the request.
- params: Query string parameters for the request.
- headers: Headers to include in the request.
- cookies: Cookies to use in the request.
- timeout: Number of seconds to wait before timing out.
- follow_redirects: Whether to follow redirects. Defaults to "safe" (rejects redirects to internal/private IPs).
- max_redirects: Maximum number of redirects. Default 30, use -1 for unlimited.
- retries: Number of retry attempts. Defaults to 3.
- retry_delay: Number of seconds to wait between retry attempts. Defaults to 1 second.
- proxies: Dict of proxies to use.
- proxy: Proxy URL to use. Format: "http://username:password@localhost:8030".
- proxy_auth: HTTP basic auth for proxy, tuple of (username, password).
- auth: HTTP basic auth tuple of (username, password). Only basic auth is supported.
- verify: Whether to verify HTTPS certificates.
- cert: Tuple of (cert, key) filenames for the client certificate.
- impersonate: Browser version to impersonate. Automatically defaults to the latest available Chrome version.
- http3: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- stealthy_headers: If enabled (default), it creates and adds real browser headers.

**Returns**:

A `Response` object.

<a id="scrapling.engines.static._ASyncSessionLogic.put"></a>

#### put

```python
def put(url: str, **kwargs: Unpack[DataRequestParams]) -> Awaitable[Response]
```

Perform a PUT request.

Any additional keyword arguments are passed to the `curl_cffi.requests.AsyncSession().request()` method.

**Arguments**:

- `url`: Target URL for the request.
- `kwargs`: Additional keyword arguments including:
- data: Form data to include in the request body.
- json: A JSON serializable object to include in the body of the request.
- params: Query string parameters for the request.
- headers: Headers to include in the request.
- cookies: Cookies to use in the request.
- timeout: Number of seconds to wait before timing out.
- follow_redirects: Whether to follow redirects. Defaults to "safe" (rejects redirects to internal/private IPs).
- max_redirects: Maximum number of redirects. Default 30, use -1 for unlimited.
- retries: Number of retry attempts. Defaults to 3.
- retry_delay: Number of seconds to wait between retry attempts. Defaults to 1 second.
- proxies: Dict of proxies to use.
- proxy: Proxy URL to use. Format: "http://username:password@localhost:8030".
- proxy_auth: HTTP basic auth for proxy, tuple of (username, password).
- auth: HTTP basic auth tuple of (username, password). Only basic auth is supported.
- verify: Whether to verify HTTPS certificates.
- cert: Tuple of (cert, key) filenames for the client certificate.
- impersonate: Browser version to impersonate. Automatically defaults to the latest available Chrome version.
- http3: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- stealthy_headers: If enabled (default), it creates and adds real browser headers.

**Returns**:

A `Response` object.

<a id="scrapling.engines.static._ASyncSessionLogic.delete"></a>

#### delete

```python
def delete(url: str,
           **kwargs: Unpack[DataRequestParams]) -> Awaitable[Response]
```

Perform a DELETE request.

Any additional keyword arguments are passed to the `curl_cffi.requests.AsyncSession().request()` method.

**Arguments**:

- `url`: Target URL for the request.
- `kwargs`: Additional keyword arguments including:
- data: Form data to include in the request body.
- json: A JSON serializable object to include in the body of the request.
- params: Query string parameters for the request.
- headers: Headers to include in the request.
- cookies: Cookies to use in the request.
- timeout: Number of seconds to wait before timing out.
- follow_redirects: Whether to follow redirects. Defaults to "safe" (rejects redirects to internal/private IPs).
- max_redirects: Maximum number of redirects. Default 30, use -1 for unlimited.
- retries: Number of retry attempts. Defaults to 3.
- retry_delay: Number of seconds to wait between retry attempts. Defaults to 1 second.
- proxies: Dict of proxies to use.
- proxy: Proxy URL to use. Format: "http://username:password@localhost:8030".
- proxy_auth: HTTP basic auth for proxy, tuple of (username, password).
- auth: HTTP basic auth tuple of (username, password). Only basic auth is supported.
- verify: Whether to verify HTTPS certificates.
- cert: Tuple of (cert, key) filenames for the client certificate.
- impersonate: Browser version to impersonate. Automatically defaults to the latest available Chrome version.
- http3: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- stealthy_headers: If enabled (default), it creates and adds real browser headers.

**Returns**:

A `Response` object.

<a id="scrapling.engines.static.FetcherSession"></a>

## FetcherSession Objects

```python
class FetcherSession()
```

A factory context manager that provides configured Fetcher sessions.

When this manager is used in a 'with' or 'async with' block,
it yields a new session configured with the manager's defaults.
A single instance of this manager should ideally be used for one active
session at a time (or sequentially). Re-entering a context with the
same manager instance while a session is already active is disallowed.

<a id="scrapling.engines.static.FetcherSession.__init__"></a>

#### \_\_init\_\_

```python
def __init__(impersonate: ImpersonateType = "chrome",
             http3: Optional[bool] = False,
             stealthy_headers: Optional[bool] = True,
             proxies: Optional[Dict[str, str]] = None,
             proxy: Optional[str] = None,
             proxy_auth: Optional[Tuple[str, str]] = None,
             timeout: Optional[int | float] = 30,
             headers: Optional[Dict[str, str]] = None,
             retries: Optional[int] = 3,
             retry_delay: Optional[int] = 1,
             follow_redirects: FollowRedirects = "safe",
             max_redirects: int = 30,
             verify: bool = True,
             cert: Optional[str | Tuple[str, str]] = None,
             selector_config: Optional[Dict] = None,
             proxy_rotator: Optional[ProxyRotator] = None)
```

**Arguments**:

- `impersonate`: Browser version to impersonate. Can be a single browser string or a list of browser strings for random selection. (Default: latest available Chrome version)
- `http3`: Whether to use HTTP3. Defaults to False. It might be problematic if used it with `impersonate`.
- `stealthy_headers`: If enabled (default), it creates and adds real browser headers. It also sets a Google referer header.
- `proxies`: Dict of proxies to use. Format: {"http": proxy_url, "https": proxy_url}.
- `proxy`: Proxy URL to use. Format: "http://username:password@localhost:8030".
Cannot be used together with the `proxies` parameter.
- `proxy_auth`: HTTP basic auth for proxy, tuple of (username, password).
- `timeout`: Number of seconds to wait before timing out.
- `headers`: Headers to include in the session with every request.
- `retries`: Number of retry attempts. Defaults to 3.
- `retry_delay`: Number of seconds to wait between retry attempts. Defaults to 1 second.
- `follow_redirects`: Whether to follow redirects. Defaults to "safe", which follows redirects but rejects those targeting internal/private IPs (SSRF protection). Pass True to follow all redirects without restriction.
- `max_redirects`: Maximum number of redirects. Default 30, use -1 for unlimited.
- `verify`: Whether to verify HTTPS certificates. Defaults to True.
- `cert`: Tuple of (cert, key) filenames for the client certificate.
- `selector_config`: Arguments passed when creating the final Selector class.
- `proxy_rotator`: A ProxyRotator instance for automatic proxy rotation.

<a id="scrapling.engines.static.FetcherSession.__enter__"></a>

#### \_\_enter\_\_

```python
def __enter__() -> _SyncSessionLogic
```

Creates and returns a new synchronous Fetcher Session

<a id="scrapling.engines.static.FetcherSession.__aenter__"></a>

#### \_\_aenter\_\_

```python
async def __aenter__() -> _ASyncSessionLogic
```

Creates and returns a new asynchronous Session.

<a id="scrapling.engines"></a>

# scrapling.engines

<a id="scrapling.fetchers.chrome"></a>

# scrapling.fetchers.chrome

<a id="scrapling.fetchers.chrome.DynamicFetcher"></a>

## DynamicFetcher Objects

```python
class DynamicFetcher(BaseFetcher)
```

A `Fetcher` that provide many options to fetch/load websites' pages through chromium-based browsers.

<a id="scrapling.fetchers.chrome.DynamicFetcher.fetch"></a>

#### fetch

```python
@classmethod
def fetch(cls, url: str, **kwargs: Unpack[PlaywrightSession]) -> Response
```

Opens up a browser and do your request based on your chosen options below.

**Arguments**:

- `url`: Target url.
- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `block_ads`: Block requests to ~3,500 known ad/tracking domains. Can be combined with ``blocked_domains``.
- `dns_over_https`: Route DNS queries through Cloudflare's DNS-over-HTTPS to prevent DNS leaks when using proxies.
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the Response object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `init_script`: An absolute path to a JavaScript file to be executed on page creation with this request.
- `locale`: Set the locale for the browser if wanted. Defaults to the system default locale.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request.
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `extra_flags`: A list of additional browser flags to pass to the browser on launch.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings.

**Returns**:

A `Response` object.

<a id="scrapling.fetchers.chrome.DynamicFetcher.async_fetch"></a>

#### async\_fetch

```python
@classmethod
async def async_fetch(cls, url: str,
                      **kwargs: Unpack[PlaywrightSession]) -> Response
```

Opens up a browser and do your request based on your chosen options below.

**Arguments**:

- `url`: Target url.
- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `block_ads`: Block requests to ~3,500 known ad/tracking domains. Can be combined with ``blocked_domains``.
- `dns_over_https`: Route DNS queries through Cloudflare's DNS-over-HTTPS to prevent DNS leaks when using proxies.
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the Response object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `init_script`: An absolute path to a JavaScript file to be executed on page creation with this request.
- `locale`: Set the locale for the browser if wanted. Defaults to the system default locale.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request.
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `extra_flags`: A list of additional browser flags to pass to the browser on launch.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings.

**Returns**:

A `Response` object.

<a id="scrapling.fetchers.chrome.PlayWrightFetcher"></a>

#### PlayWrightFetcher

For backward-compatibility

<a id="scrapling.fetchers.stealth_chrome"></a>

# scrapling.fetchers.stealth\_chrome

<a id="scrapling.fetchers.stealth_chrome.StealthyFetcher"></a>

## StealthyFetcher Objects

```python
class StealthyFetcher(BaseFetcher)
```

A `Fetcher` class type which is a completely stealthy built on top of Chromium.

It works as real browsers passing almost all online tests/protections with many customization options.

<a id="scrapling.fetchers.stealth_chrome.StealthyFetcher.fetch"></a>

#### fetch

```python
@classmethod
def fetch(cls, url: str, **kwargs: Unpack[StealthSession]) -> Response
```

Opens up a browser and do your request based on your chosen options below.

**Arguments**:

- `url`: Target url.
- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `block_ads`: Block requests to ~3,500 known ad/tracking domains. Can be combined with ``blocked_domains``.
- `dns_over_https`: Route DNS queries through Cloudflare's DNS-over-HTTPS to prevent DNS leaks when using proxies.
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `init_script`: An absolute path to a JavaScript file to be executed on page creation for all pages in this session.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `solve_cloudflare`: Solves all types of the Cloudflare's Turnstile/Interstitial challenges before returning the response to you.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `hide_canvas`: Add random noise to canvas operations to prevent fingerprinting.
- `block_webrtc`: Forces WebRTC to respect proxy settings to prevent local IP address leak.
- `allow_webgl`: Enabled by default. Disabling it disables WebGL and WebGL 2.0 support entirely. Disabling WebGL is not recommended as many WAFs now check if WebGL is enabled.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `user_data_dir`: Path to a User Data Directory, which stores browser session data like cookies and local storage. The default is to create a temporary directory.
- `extra_flags`: A list of additional browser flags to pass to the browser on launch.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings, and it takes higher priority than Scrapling's settings.

**Returns**:

A `Response` object.

<a id="scrapling.fetchers.stealth_chrome.StealthyFetcher.async_fetch"></a>

#### async\_fetch

```python
@classmethod
async def async_fetch(cls, url: str,
                      **kwargs: Unpack[StealthSession]) -> Response
```

Opens up a browser and do your request based on your chosen options below.

**Arguments**:

- `url`: Target url.
- `headless`: Run the browser in headless/hidden (default), or headful/visible mode.
- `disable_resources`: Drop requests for unnecessary resources for a speed boost.
Requests dropped are of type `font`, `image`, `media`, `beacon`, `object`, `imageset`, `texttrack`, `websocket`, `csp_report`, and `stylesheet`.
- `blocked_domains`: A set of domain names to block requests to. Subdomains are also matched (e.g., ``"example.com"`` blocks ``"sub.example.com"`` too).
- `block_ads`: Block requests to ~3,500 known ad/tracking domains. Can be combined with ``blocked_domains``.
- `dns_over_https`: Route DNS queries through Cloudflare's DNS-over-HTTPS to prevent DNS leaks when using proxies.
- `useragent`: Pass a useragent string to be used. Otherwise the fetcher will generate a real Useragent of the same browser and use it.
- `cookies`: Set cookies for the next request.
- `network_idle`: Wait for the page until there are no network connections for at least 500 ms.
- `timeout`: The timeout in milliseconds that is used in all operations and waits through the page. The default is 30,000
- `wait`: The time (milliseconds) the fetcher will wait after everything finishes before closing the page and returning the ` Response ` object.
- `page_action`: Added for automation. A function that takes the `page` object, runs after navigation, and does the automation you need.
- `page_setup`: A function that takes the `page` object, runs before navigation. Use it to register event listeners or routes that must be set up before the page loads.
- `wait_selector`: Wait for a specific CSS selector to be in a specific state.
- `init_script`: An absolute path to a JavaScript file to be executed on page creation for all pages in this session.
- `locale`: Specify user locale, for example, `en-GB`, `de-DE`, etc. Locale will affect navigator.language value, Accept-Language request header value as well as number and date formatting
rules. Defaults to the system default locale.
- `timezone_id`: Changes the timezone of the browser. Defaults to the system timezone.
- `wait_selector_state`: The state to wait for the selector given with `wait_selector`. The default state is `attached`.
- `solve_cloudflare`: Solves all types of the Cloudflare's Turnstile/Interstitial challenges before returning the response to you.
- `real_chrome`: If you have a Chrome browser installed on your device, enable this, and the Fetcher will launch an instance of your browser and use it.
- `hide_canvas`: Add random noise to canvas operations to prevent fingerprinting.
- `block_webrtc`: Forces WebRTC to respect proxy settings to prevent local IP address leak.
- `allow_webgl`: Enabled by default. Disabling it disables WebGL and WebGL 2.0 support entirely. Disabling WebGL is not recommended as many WAFs now check if WebGL is enabled.
- `load_dom`: Enabled by default, wait for all JavaScript on page(s) to fully load and execute.
- `cdp_url`: Instead of launching a new browser instance, connect to this CDP URL to control real browsers through CDP.
- `google_search`: Enabled by default, Scrapling will set a Google referer header.
- `extra_headers`: A dictionary of extra headers to add to the request. _The referer set by `google_search` takes priority over the referer set here if used together._
- `proxy`: The proxy to be used with requests, it can be a string or a dictionary with the keys 'server', 'username', and 'password' only.
- `user_data_dir`: Path to a User Data Directory, which stores browser session data like cookies and local storage. The default is to create a temporary directory.
- `extra_flags`: A list of additional browser flags to pass to the browser on launch.
- `selector_config`: The arguments that will be passed in the end while creating the final Selector's class.
- `additional_args`: Additional arguments to be passed to Playwright's context as additional settings, and it takes higher priority than Scrapling's settings.

**Returns**:

A `Response` object.

<a id="scrapling.fetchers.requests"></a>

# scrapling.fetchers.requests

<a id="scrapling.fetchers.requests.Fetcher"></a>

## Fetcher Objects

```python
class Fetcher(BaseFetcher)
```

A basic `Fetcher` class type that can only do basic GET, POST, PUT, and DELETE HTTP requests based on `curl_cffi`.

<a id="scrapling.fetchers.requests.AsyncFetcher"></a>

## AsyncFetcher Objects

```python
class AsyncFetcher(BaseFetcher)
```

A basic `Fetcher` class type that can only do basic GET, POST, PUT, and DELETE HTTP requests based on `curl_cffi`.

<a id="scrapling.fetchers"></a>

# scrapling.fetchers

<a id="scrapling.fetchers.__dir__"></a>

#### \_\_dir\_\_

```python
def __dir__() -> list[str]
```

Support for dir() and autocomplete.

