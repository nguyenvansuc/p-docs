# Hướng Dẫn Tích Hợp SEO React Plugin

## Tổng Quan

SEO React Plugin là plugin giao diện dùng trong màn hình biên tập bài viết. Mục đích của plugin là hỗ trợ biên tập viên kiểm tra và tối ưu SEO trước khi xuất bản.

Plugin phục vụ các tính năng chính:

- Chấm điểm SEO bài viết theo tiêu đề, mô tả, từ khóa, heading, nội dung, liên kết và hình ảnh.
- Hiển thị danh sách tiêu chí đã đạt và chưa đạt.
- Gợi ý từ khóa chủ đạo, tiêu đề SEO, mô tả SEO và tags.
- Áp dụng gợi ý ngược về hệ thống đang tích hợp.
- Tìm kiếm, thêm và xóa tags.
- Đồng bộ alt còn thiếu cho ảnh theo tiêu đề SEO.
- Gửi điểm SEO hiện tại ra ngoài để hệ thống tích hợp hiển thị hoặc lưu lại.

Plugin được build thành một file JavaScript và khai báo biến global:

<div style="display:flex;align-items:center;gap:6px;flex-wrap:wrap;">
  <code id="seo-plugin-copy-mount-api">window.SeoEditorPluginIframe.mountSeoPlugin(options)</code>
  <button type="button" title="Sao chép hàm mount" aria-label="Sao chép hàm mount" onclick="navigator.clipboard.writeText(document.getElementById('seo-plugin-copy-mount-api').innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;">
    <svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;">
      <rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect>
      <path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path>
    </svg>
  </button>
</div>

Lập trình viên tích hợp không cần biết phần React bên trong plugin. Chỉ cần hiểu cách truyền dữ liệu qua `source.get()` và `source.set(value)`.

## Cách Plugin Hoạt Động

Plugin không sở hữu dữ liệu bài viết. Hệ thống tích hợp vẫn là nơi quản lý dữ liệu thật.

Plugin chỉ làm 3 việc:

1. Đọc dữ liệu hiện tại từ hệ thống tích hợp bằng `source[field].get()`.
2. Phân tích SEO và render giao diện.
3. Khi người dùng áp dụng gợi ý, ghi dữ liệu về hệ thống tích hợp bằng `source[field].set(value)`.

Hệ thống tích hợp cần làm 3 việc:

1. Cung cấp dữ liệu hiện tại cho plugin qua `source`.
2. Cập nhật dữ liệu thật khi plugin gọi `set(value)`.
3. Gọi `instance.refresh()` khi dữ liệu bài viết thay đổi.

Luồng tổng quát:

```txt
Hệ thống tích hợp/editor
  -> plugin gọi source.get()
  -> plugin phân tích SEO và hiển thị UI
  -> người dùng chọn gợi ý
  -> plugin gọi source.set(value)
  -> hệ thống tích hợp cập nhật dữ liệu thật
  -> hệ thống tích hợp gọi refresh khi dữ liệu đổi
```

## Cài Đặt

Tải file bundle của plugin:

<div style="display:flex;align-items:center;gap:6px;flex-wrap:wrap;">
  <code id="seo-plugin-copy-script">&lt;script src="https://ims.mediacdn.vn/plugins/seo/seo-plugin-react.min.js"&gt;&lt;/script&gt;</code>
  <button type="button" title="Sao chép script" aria-label="Sao chép script" onclick="navigator.clipboard.writeText(document.getElementById('seo-plugin-copy-script').innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;">
    <svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;">
      <rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect>
      <path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path>
    </svg>
  </button>
</div>

Sau khi tải script, biến global này phải tồn tại:

<div style="display:flex;align-items:center;gap:6px;flex-wrap:wrap;">
  <code id="seo-plugin-copy-global">window.SeoEditorPluginIframe</code>
  <button type="button" title="Sao chép biến global" aria-label="Sao chép biến global" onclick="navigator.clipboard.writeText(document.getElementById('seo-plugin-copy-global').innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;">
    <svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;">
      <rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect>
      <path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path>
    </svg>
  </button>
</div>

## Tích Hợp Nhanh <button type="button" title="Sao chép mẫu tích hợp nhanh" aria-label="Sao chép mẫu tích hợp nhanh" onclick="navigator.clipboard.writeText(document.getElementById('seo-plugin-copy-quick').innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;"><rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect><path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path></svg></button>

Bấm icon copy bên cạnh tiêu đề để sao chép toàn bộ mẫu tích hợp, sau đó thay các phần có ghi chú `Logic hệ thống tích hợp tự viết` bằng logic thật của hệ thống.

<pre id="seo-plugin-copy-quick"><code>&lt;div id="seo-panel"&gt;&lt;/div&gt;

&lt;script src="https://ims.mediacdn.vn/plugins/seo/seo-plugin-react.min.js"&gt;&lt;/script&gt;
&lt;script&gt;
  // Logic hệ thống tích hợp tự viết: nơi lưu dữ liệu SEO thật.
  var seoData = {
    title: "",
    keyword: "",
    description: "",
    tags: [],
  };

  var instance = window.SeoEditorPluginIframe.mountSeoPlugin({
    target: document.querySelector("#seo-panel"),
    config: {
      // Bắt buộc của plugin: source là nơi plugin đọc và ghi dữ liệu.
      source: {
        html: {
          get: function () {
            // Logic hệ thống tích hợp tự viết: lấy HTML từ editor.
            return editor.getData();
          },
        },
        title: {
          get: function () {
            return seoData.title;
          },
          set: function (value) {
            // Logic hệ thống tích hợp tự viết: cập nhật title thật.
            seoData.title = value || "";
          },
        },
        keyword: {
          get: function () {
            return seoData.keyword;
          },
          set: function (value) {
            // Logic hệ thống tích hợp tự viết: cập nhật keyword thật.
            seoData.keyword = value || "";
          },
        },
        description: {
          get: function () {
            return seoData.description;
          },
          set: function (value) {
            // Logic hệ thống tích hợp tự viết: cập nhật mô tả thật.
            seoData.description = value || "";
          },
        },
      },
    },
  });

  // Logic hệ thống tích hợp tự viết: khi editor đổi thì gọi refresh.
  editor.model.document.on("change:data", function () {
    instance.refresh();
  });
&lt;/script&gt;</code></pre>

## Ví Dụ Tối Thiểu

HTML:

<div style="display:flex;align-items:center;gap:6px;flex-wrap:wrap;">
  <code id="seo-plugin-copy-panel">&lt;div id="seo-panel"&gt;&lt;/div&gt;</code>
  <button type="button" title="Sao chép HTML" aria-label="Sao chép HTML" onclick="navigator.clipboard.writeText(document.getElementById('seo-plugin-copy-panel').innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;">
    <svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;">
      <rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect>
      <path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path>
    </svg>
  </button>
</div>

JavaScript <button type="button" title="Sao chép JavaScript" aria-label="Sao chép JavaScript" onclick="navigator.clipboard.writeText(this.parentElement.nextElementSibling.innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;"><rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect><path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path></svg></button>:

```js
// Logic hệ thống tích hợp tự viết: nơi lưu dữ liệu SEO thật của bài viết.
var seoData = {
  title: "",
  keyword: "",
  description: "",
  tags: [],
};

var instance = window.SeoEditorPluginIframe.mountSeoPlugin({
  target: document.querySelector("#seo-panel"),
  config: {
    // Bắt buộc của plugin: source là nơi plugin đọc và ghi dữ liệu.
    source: {
      html: {
        // Bắt buộc của plugin: trả HTML hiện tại của bài viết.
        get: function () {
          // Logic hệ thống tích hợp tự viết: lấy HTML từ editor đang dùng.
          return editor.getData();
        },
      },
      title: {
        get: function () {
          return seoData.title;
        },
        set: function (value) {
          // Logic hệ thống tích hợp tự viết: cập nhật dữ liệu thật của hệ thống.
          seoData.title = value || "";
        },
      },
      keyword: {
        get: function () {
          return seoData.keyword;
        },
        set: function (value) {
          seoData.keyword = value || "";
        },
      },
      description: {
        get: function () {
          return seoData.description;
        },
        set: function (value) {
          seoData.description = value || "";
        },
      },
    },
  },
});

// Logic hệ thống tích hợp tự viết: khi nội dung editor đổi thì báo plugin đọc lại dữ liệu.
editor.model.document.on("change:data", function () {
  instance.refresh();
});
```

## Trách Nhiệm Khi Tích Hợp

### Plugin cần dữ liệu gì?

| Dữ liệu đầu vào | Bắt buộc | Mục đích |
| --- | --- | --- |
| `source.html.get()` | Có | Lấy HTML bài viết để chấm nội dung, heading, link và ảnh |
| `source.title.get()` | Nên có | Lấy tiêu đề SEO để chấm nhóm tiêu đề |
| `source.keyword.get()` | Nên có | Lấy từ khóa chủ đạo để chấm keyword |
| `source.description.get()` | Nên có | Lấy mô tả SEO để chấm nhóm mô tả |
| `source.tags.get()` | Không | Lấy tags hiện tại |
| `config.domain` | Không | Phân biệt link nội bộ và link ngoài |
| `config.ruleConfig` | Không | Bật/tắt rule và chỉnh ngưỡng chấm điểm |
| `config.ui` | Không | Ẩn/hiện một số phần giao diện |

### Lập trình viên tích hợp phải viết gì?

| Phần cần viết | Khi nào cần | Mục đích |
| --- | --- | --- |
| `source.html.get` | Luôn nên có | Trả HTML hiện tại từ editor |
| `source.title.get/set` | Khi có tiêu đề SEO | Đọc/ghi tiêu đề SEO |
| `source.keyword.get/set` | Khi có từ khóa chủ đạo | Đọc/ghi từ khóa chủ đạo |
| `source.description.get/set` | Khi có mô tả SEO | Đọc/ghi mô tả SEO |
| `source.tags.get/set` | Khi bật field tags | Đọc/ghi tags |
| `source.syncCorrectImageAlt.set` | Khi dùng tính năng sửa alt ảnh | Ghi HTML đã sửa alt vào editor |
| `config.api.getFieldSuggestions` | Khi muốn có gợi ý AI | Trả gợi ý keyword/title/description/tags |
| `config.api.searchTags` | Khi muốn tìm kiếm tags | Trả danh sách tag theo từ khóa tìm kiếm |
| Listener gọi `refresh()` | Luôn nên có | Báo plugin đọc lại dữ liệu khi editor/form đổi |

## API Chính

### `mountSeoPlugin(options)`

Gọi trực tiếp bằng object cấu hình:

```js
var instance = window.SeoEditorPluginIframe.mountSeoPlugin({
  // Bắt buộc nếu muốn plugin render vào một vị trí có sẵn trên trang.
  target: document.querySelector("#seo-panel"),

  // Bắt buộc: toàn bộ cấu hình plugin nằm trong config.
  config: {
    source: source,
    api: api,
    domain: "https://example.com",
    ruleConfig: ruleConfig,
    ui: uiConfig,
    onScoreChange: function (score, result) {
      // Logic hệ thống tích hợp tự viết: nhận điểm SEO nếu cần.
    },
  },
});
```

Ý nghĩa:

- `instance.refresh()`: bắt plugin đọc lại dữ liệu và tính lại điểm.
- `instance.destroy()`: hủy plugin và gỡ UI.
- `instance.iframe`: iframe hiện tại của plugin.
- Nếu không truyền `target`, plugin sẽ tự mở theo chế độ popup.

## Cấu Hình Plugin

Ví dụ cấu hình đầy đủ:

```js
window.SeoEditorPluginIframe.mountSeoPlugin({
  target: document.querySelector("#seo-panel"),
  config: {
    source: source,
    api: api,
    domain: "https://example.com",
    ruleConfig: ruleConfig,
    ui: uiConfig,
    onScoreChange: function (score, result) {},
  },
});
```

### `config.source`

`source` là giao ước quan trọng nhất. Đây là nơi plugin đọc và ghi dữ liệu.

```js
var source = {
  html: {
    get: function () {
      // Logic hệ thống tích hợp tự viết: lấy HTML hiện tại từ editor.
      return editor.getData();
    },
  },
  title: {
    get: function () {
      return seoData.title;
    },
    set: function (value) {
      // Logic hệ thống tích hợp tự viết: ghi title mới về dữ liệu thật.
      seoData.title = value || "";
    },
  },
  keyword: {
    get: function () {
      return seoData.keyword;
    },
    set: function (value) {
      seoData.keyword = value || "";
    },
  },
  description: {
    get: function () {
      return seoData.description;
    },
    set: function (value) {
      seoData.description = value || "";
    },
  },
  tags: {
    get: function () {
      return seoData.tags;
    },
    set: function (value) {
      seoData.tags = value || [];
    },
  },
  syncCorrectImageAlt: {
    set: function (html) {
      // Logic hệ thống tích hợp tự viết: ghi HTML đã sửa alt vào editor.
      editor.setData(html || "");
    },
  },
};
```

Nếu thiếu field, plugin hiểu giá trị field đó là rỗng. Nếu thiếu `set`, plugin vẫn hiển thị nhưng không thể ghi ngược field đó khi người dùng áp dụng gợi ý.

### `config.api`

`api` là nơi hệ thống tích hợp cung cấp các hàm gợi ý hoặc tìm kiếm tags.

```js
var api = {
  getFieldSuggestions: function (field, payload) {
    // Logic hệ thống tích hợp tự viết: gọi API gợi ý keyword/title/description/tags.
    if (field === "keyword") {
      return [
        { key: "tối ưu SEO", label: "tối ưu SEO" },
        { key: "SEO bài viết", label: "SEO bài viết" },
      ];
    }

    if (field === "title") {
      return [
        {
          key: payload.keyword + ": những điểm cần tối ưu trước khi xuất bản",
          label: payload.keyword + ": những điểm cần tối ưu trước khi xuất bản",
        },
      ];
    }

    return [];
  },
  searchTags: function (payload, query) {
    // Logic hệ thống tích hợp tự viết: gọi API tìm tag theo từ khóa người dùng nhập.
    return [
      { key: 101, label: "SEO nội dung" },
      { key: 102, label: "Google Search" },
    ].filter(function (tag) {
      return tag.label.toLowerCase().indexOf((query || "").toLowerCase()) !== -1;
    });
  },
};
```

Định dạng gợi ý:

```js
[
  {
    key: "giá trị sẽ ghi vào source",
    label: "text hiển thị trên UI",
  },
]
```

Plugin ghi `key`, không ghi `label`.

Định dạng tags:

```js
[
  { key: 101, label: "SEO nội dung" },
  { key: 102, label: "Google Search" },
]
```

Plugin cũng chấp nhận định dạng legacy:

```js
[
  { Id: 101, Name: "SEO nội dung" },
  { TagId: 102, Name: "Google Search" },
]
```

### `config.domain`

Dùng để phân biệt link nội bộ và link ngoài.

```js
domain: "https://thanhnien.vn"
```

Nếu không truyền `domain`, plugin vẫn chạy, nhưng rule link nội bộ/link ngoài có thể kém chính xác hơn.

### `config.onScoreChange`

Hàm callback khi điểm SEO thay đổi.

```js
onScoreChange: function (score, result) {
  console.log(score, result);
}
```

`score` là số từ 0 đến 100.

`result` gồm điểm, level và danh sách checks.

### `config.ui`

Dùng để ẩn/hiện UI.

```js
ui: {
  visibility: {
    contentTab: true,
    improveTab: true,
    keyword: true,
    title: true,
    description: true,
    tags: false,
  },
}
```

Mặc định field nào không khai báo thì được hiển thị.

## Payload Truyền Vào API

Khi plugin gọi `getFieldSuggestions` hoặc `searchTags`, plugin truyền `payload`.

```js
var payload = {
  html: "<p>Nội dung bài viết...</p>",
  text: "Nội dung bài viết...",
  title: "Tiêu đề SEO hiện tại",
  description: "Mô tả SEO hiện tại",
  keyword: "từ khóa chủ đạo",
  tags: [
    { key: 101, label: "SEO nội dung" },
  ],
  domain: "https://example.com",
  ruleConfig: ruleConfig,
  ui: uiConfig,
};
```

Ví dụ:

```js
function getFieldSuggestions(field, payload) {
  if (field === "title") {
    return [
      {
        key: payload.keyword + ": những điểm cần tối ưu trước khi xuất bản",
        label: payload.keyword + ": những điểm cần tối ưu trước khi xuất bản",
      },
    ];
  }

  return [];
}
```

## Muốn Có Tính Năng Nào Thì Cần Tích Hợp Gì?

Đọc bảng này theo cách: nếu màn hình của bạn cần tính năng ở cột bên trái, hãy tích hợp đủ các phần ở cột bên phải.

| Muốn có tính năng | Cần tích hợp |
| --- | --- |
| Chấm điểm SEO cho nội dung bài viết | `source.html.get()` để plugin đọc HTML bài viết |
| Chấm tiêu đề SEO | `source.title.get()` để plugin đọc tiêu đề hiện tại |
| Chấm mô tả SEO | `source.description.get()` để plugin đọc mô tả hiện tại |
| Chấm từ khóa chủ đạo trong title/mô tả/nội dung | `source.keyword.get()` để plugin đọc từ khóa hiện tại |
| Cập nhật điểm khi người dùng sửa bài | Sau khi editor/form đổi, hệ thống tích hợp gọi `instance.refresh()` |
| Cho phép plugin áp dụng gợi ý title | `source.title.set(value)` để ghi title mới về hệ thống |
| Cho phép plugin áp dụng gợi ý keyword | `source.keyword.set(value)` để ghi keyword mới về hệ thống |
| Cho phép plugin áp dụng gợi ý description | `source.description.set(value)` để ghi mô tả mới về hệ thống |
| Hiển thị nút gợi ý AI cho keyword/title/description | `config.api.getFieldSuggestions(field, payload)` |
| Hiển thị tags hiện tại | `source.tags.get()` |
| Thêm/xóa tags | `source.tags.set(value)` |
| Tìm kiếm tags khi người dùng nhập | `config.api.searchTags(payload, query)` |
| Gợi ý tags bằng nút "Gợi ý" | `config.api.getFieldSuggestions("tags", payload)` |
| Check link nội bộ/link ngoài chính xác | `config.domain` |
| Đồng bộ alt ảnh còn thiếu | Bật rule `CONTENT_IMAGE_ALT` và khai báo `source.syncCorrectImageAlt.set(html)` |
| Hiển thị điểm SEO ở ngoài plugin | `config.onScoreChange(score, result)` |
| Ẩn/hiện field hoặc tab | `config.ui.visibility` |

Nếu chỉ muốn chấm điểm cơ bản, bạn thường chỉ cần:

```js
source.html.get()
source.title.get()
source.keyword.get()
source.description.get()
instance.refresh()
```

Nếu muốn người dùng áp dụng gợi ý, bạn cần thêm các hàm `set(value)` tương ứng.

## Chi Tiết Tính Năng

### 1. Chấm Điểm SEO

Mục đích: chấm điểm bài viết từ 0 đến 100.

Plugin đọc:

- `source.html.get()`
- `source.title.get()`
- `source.keyword.get()`
- `source.description.get()`
- `config.domain`
- `config.ruleConfig`

Hệ thống tích hợp cần gọi `refresh()` khi dữ liệu đổi:

```js
editor.model.document.on("change:data", function () {
  instance.refresh();
});
```

Nếu hệ thống tích hợp không gọi `refresh()`, điểm SEO có thể không cập nhật khi bài viết đổi.

### 2. Tab Cải Thiện

Mục đích: hiển thị tiêu chí đã đạt và chưa đạt.

Plugin tự tạo dữ liệu tab này từ kết quả phân tích SEO. Hệ thống tích hợp không cần viết API riêng.

Cần:

- Dữ liệu `source` đúng.
- `ruleConfig.criteria` nếu muốn bật/tắt tiêu chí.
- `ui.visibility.improveTab !== false`.

### 3. Gợi Ý Field

Mục đích: người dùng bấm "Gợi ý" để lấy gợi ý keyword/title/description/tags.

Cần:

```js
api: {
  getFieldSuggestions: function (field, payload) {
    return [];
  },
}
```

Ví dụ:

```js
function getFieldSuggestions(field, payload) {
  if (field === "keyword") {
    return [
      { key: "tối ưu SEO", label: "tối ưu SEO" },
      { key: "SEO bài viết", label: "SEO bài viết" },
    ];
  }

  if (field === "title") {
    return [
      {
        key: "Cách tối ưu " + payload.keyword + " cho bài viết tin tức",
        label: "Cách tối ưu " + payload.keyword + " cho bài viết tin tức",
      },
    ];
  }

  return [];
}
```

Để người dùng áp dụng được gợi ý, field tương ứng phải có `set`.

```js
title: {
  get: function () {
    return seoData.title;
  },
  set: function (value) {
    seoData.title = value || "";
  },
}
```

### 4. Tags

Mục đích:

- Hiển thị tags hiện tại.
- Thêm tag từ gợi ý.
- Tìm tag khi người dùng nhập.
- Xóa tag.

Cần:

```js
source: {
  tags: {
    get: function () {
      return seoData.tags;
    },
    set: function (value) {
      seoData.tags = value || [];
    },
  },
},
api: {
  searchTags: function (payload, query) {
    return [
      { key: 101, label: "SEO kỹ thuật" },
      { key: 102, label: "SEO nội dung" },
    ];
  },
}
```

Nếu muốn nút "Gợi ý" của tags trả về tags, thêm:

```js
api: {
  getFieldSuggestions: function (field, payload) {
    if (field === "tags") {
      return [
        { key: 201, label: "Google Search" },
      ];
    }
    return [];
  },
}
```

### 5. Đồng Bộ Alt Ảnh Còn Thiếu

Mục đích: khi bài viết có ảnh thiếu alt, plugin tạo HTML mới đã bổ sung alt theo tiêu đề SEO.

Cần bật rule:

```js
ruleConfig: {
  criteria: {
    CONTENT_IMAGE_ALT: true,
  },
}
```

Cần khai báo writer:

```js
syncCorrectImageAlt: {
  set: function (html) {
    editor.setData(html || "");
  },
}
```

Nếu thiếu `syncCorrectImageAlt.set`, nút sửa alt không ghi được vào editor.

### 6. Nhận Điểm SEO Ở Bên Ngoài

Mục đích: hệ thống tích hợp nhận điểm SEO để hiển thị ở nơi khác hoặc lưu state.

Cần:

```js
onScoreChange: function (score, result) {
  scoreElement.textContent = score + "/100";
}
```

Hàm callback chỉ chạy khi điểm thay đổi.

### 7. Ẩn/Hiện UI

Mục đích: ẩn field không dùng trong màn hình tích hợp.

Cần:

```js
ui: {
  visibility: {
    tags: false,
    description: true,
    improveTab: true,
  },
}
```

## Rule Config

`ruleConfig` gồm 2 phần:

- `settings`: chỉnh cách chấm và ngưỡng.
- `criteria`: bật/tắt từng tiêu chí.

Ví dụ cấu hình thường dùng:

```js
var ruleConfig = {
  settings: {
    ignoreCharacters: true,
    ignoreSeoHeading: false,
    maxWordsOfTitle: 18,
    maxWordsOfSapo: 50,
    ignoreTooLongSentences: false,
    ignoreExternalLinks: false,
    ignoreCheckmissingAlt: false,
    ignorePercentTransitionWord: false,
    ignoreKeywordInAlt: false,
    ignorePercentKeywordCount: false,
    ignoreHeadingH3ToH6: false,
    usePercentKeywordCount: false,
  },
  criteria: {
    TITLE_KEYWORD: true,
    TITLE_WORDS: true,
    DESCRIPTION_KEYWORD: true,
    DESCRIPTION_WORDS: true,
    CONTENT_KEYWORD: true,
    CONTENT_WORD_COUNT: true,
    CONTENT_PARAGRAPH_LENGTH: true,
    HEADING_H2_EXISTS: true,
    HEADING_H2_KEYWORD: true,

    TITLE_CHARACTERS: false,
    DESCRIPTION_CHARACTERS: false,
    HEADING_H3_H6_EXISTS: false,
    HEADING_H3_H6_KEYWORD: false,
    PRIMARY_KEYWORD_EXISTS: false,
    CONTENT_FIRST_PARAGRAPH_KEYWORD: false,
    CONTENT_PERCENT_KEYWORD: false,
    CONTENT_LONG_SENTENCES: false,
    CONTENT_TRANSITION_WORD: false,
    CONTENT_INTERNAL_LINK: false,
    CONTENT_IMAGE_EXISTS: false,
    CONTENT_IMAGE_ALT: false,
    CONTENT_KEYWORD_IN_ALT: false,
    CONTENT_EXTERNAL_LINK: false,
  },
};
```

Ghi nhớ:

- `criteria.KEY = false` nghĩa là rule đó không hiển thị và không tính điểm.
- `ignoreSeoHeading: true` bỏ qua nhóm heading và phân bổ lại trọng số điểm.
- `maxWordsOfTitle` và `maxWordsOfSapo` là ngưỡng số từ.

## Các Bước Tích Hợp

### Bước 1: Thêm vị trí render plugin

<div style="display:flex;align-items:center;gap:6px;flex-wrap:wrap;">
  <code id="seo-plugin-copy-step-panel">&lt;aside id="seo-panel"&gt;&lt;/aside&gt;</code>
  <button type="button" title="Sao chép vị trí render" aria-label="Sao chép vị trí render" onclick="navigator.clipboard.writeText(document.getElementById('seo-plugin-copy-step-panel').innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;">
    <svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;">
      <rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect>
      <path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path>
    </svg>
  </button>
</div>

### Bước 2: Tải script plugin

<div style="display:flex;align-items:center;gap:6px;flex-wrap:wrap;">
  <code id="seo-plugin-copy-step-script">&lt;script src="https://ims.mediacdn.vn/plugins/seo/seo-plugin-react.min.js"&gt;&lt;/script&gt;</code>
  <button type="button" title="Sao chép script" aria-label="Sao chép script" onclick="navigator.clipboard.writeText(document.getElementById('seo-plugin-copy-step-script').innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;">
    <svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;">
      <rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect>
      <path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path>
    </svg>
  </button>
</div>

### Bước 3: Chuẩn bị `source`

```js
var source = {
  html: {
    get: function () {
      return editor.getData();
    },
  },
  title: {
    get: function () {
      return seoData.title;
    },
    set: function (value) {
      seoData.title = value || "";
    },
  },
  keyword: {
    get: function () {
      return seoData.keyword;
    },
    set: function (value) {
      seoData.keyword = value || "";
    },
  },
  description: {
    get: function () {
      return seoData.description;
    },
    set: function (value) {
      seoData.description = value || "";
    },
  },
};
```

### Bước 4: Mount plugin

```js
var instance = window.SeoEditorPluginIframe.mountSeoPlugin({
  target: document.querySelector("#seo-panel"),
  config: {
    source: source,
    api: {
      getFieldSuggestions: getFieldSuggestions,
      searchTags: searchTags,
    },
    ruleConfig: ruleConfig,
  },
});
```

### Bước 5: Gọi refresh khi dữ liệu đổi

```js
editor.model.document.on("change:data", function () {
  instance.refresh();
});
```

Nếu title, keyword, description được sửa ở ngoài plugin, hệ thống tích hợp cũng cần gọi `refresh()`.

```js
function onSeoFormChanged() {
  instance.refresh();
}
```

### Bước 6: Hủy plugin khi màn hình unmount

```js
instance.destroy();
```

## Ví Dụ CKEditor <button type="button" title="Sao chép ví dụ CKEditor" aria-label="Sao chép ví dụ CKEditor" onclick="navigator.clipboard.writeText(this.parentElement.nextElementSibling.innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;"><rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect><path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path></svg></button>

```html
<div id="seo-panel"></div>

<script src="https://ims.mediacdn.vn/plugins/seo/seo-plugin-react.min.js"></script>
<script>
  // Logic hệ thống tích hợp tự viết: nơi lưu dữ liệu SEO thật.
  var seoData = {
    title: "",
    keyword: "",
    description: "",
    tags: [],
  };

  ClassicEditor.create(document.querySelector("#content")).then(function (editor) {
    var instance = window.SeoEditorPluginIframe.mountSeoPlugin({
      target: document.querySelector("#seo-panel"),
      config: {
        // Bắt buộc của plugin: khai báo source để plugin đọc/ghi dữ liệu.
        source: {
          html: {
            get: function () {
              // Logic hệ thống tích hợp tự viết: lấy HTML từ CKEditor.
              return editor.getData();
            },
          },
          title: {
            get: function () {
              return seoData.title;
            },
            set: function (value) {
              // Logic hệ thống tích hợp tự viết: ghi title mới vào dữ liệu thật.
              seoData.title = value || "";
            },
          },
          keyword: {
            get: function () {
              return seoData.keyword;
            },
            set: function (value) {
              seoData.keyword = value || "";
            },
          },
          description: {
            get: function () {
              return seoData.description;
            },
            set: function (value) {
              seoData.description = value || "";
            },
          },
          tags: {
            get: function () {
              return seoData.tags;
            },
            set: function (value) {
              seoData.tags = value || [];
            },
          },
          syncCorrectImageAlt: {
            set: function (html) {
              // Logic hệ thống tích hợp tự viết: ghi HTML đã sửa alt vào CKEditor.
              editor.setData(html || "");
            },
          },
        },
        api: {
          // Logic hệ thống tích hợp tự viết: gọi API gợi ý của hệ thống.
          getFieldSuggestions: getFieldSuggestions,
          // Logic hệ thống tích hợp tự viết: gọi API tìm kiếm tags của hệ thống.
          searchTags: searchTags,
        },
        domain: "https://example.com",
        ruleConfig: ruleConfig,
      },
    });

    // Logic hệ thống tích hợp tự viết: khi editor đổi thì báo plugin tính lại.
    editor.model.document.on("change:data", function () {
      instance.refresh();
    });
  });
</script>
```

## Ví Dụ Quill <button type="button" title="Sao chép ví dụ Quill" aria-label="Sao chép ví dụ Quill" onclick="navigator.clipboard.writeText(this.parentElement.nextElementSibling.innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;"><rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect><path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path></svg></button>

```js
// Logic hệ thống tích hợp tự viết: khởi tạo Quill.
var quill = new Quill("#editor", { theme: "snow" });

var instance = window.SeoEditorPluginIframe.mountSeoPlugin({
  target: document.querySelector("#seo-panel"),
  config: {
    // Bắt buộc của plugin: khai báo source để plugin đọc/ghi dữ liệu.
    source: {
      html: {
        get: function () {
          // Logic hệ thống tích hợp tự viết: lấy HTML từ Quill.
          return quill.root.innerHTML;
        },
      },
      title: {
        get: function () {
          return seoData.title;
        },
        set: function (value) {
          seoData.title = value || "";
        },
      },
      keyword: {
        get: function () {
          return seoData.keyword;
        },
        set: function (value) {
          seoData.keyword = value || "";
        },
      },
      description: {
        get: function () {
          return seoData.description;
        },
        set: function (value) {
          seoData.description = value || "";
        },
      },
      syncCorrectImageAlt: {
        set: function (html) {
          // Logic hệ thống tích hợp tự viết: ghi HTML đã sửa alt vào Quill.
          quill.clipboard.dangerouslyPasteHTML(html || "");
        },
      },
    },
  },
});

// Logic hệ thống tích hợp tự viết: khi editor đổi thì báo plugin tính lại.
quill.on("text-change", function () {
  instance.refresh();
});
```

## Ví Dụ React State <button type="button" title="Sao chép ví dụ React State" aria-label="Sao chép ví dụ React State" onclick="navigator.clipboard.writeText(this.parentElement.nextElementSibling.innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;"><rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect><path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path></svg></button>

```jsx
import { useEffect, useRef, useState } from "react";

export default function SeoPanel() {
  // Logic hệ thống tích hợp tự viết: DOM để plugin render vào.
  const panelRef = useRef(null);
  const pluginRef = useRef(null);
  const latestStateRef = useRef(null);
  // Logic hệ thống tích hợp tự viết: state thật của màn hình biên tập.
  const [state, setState] = useState({
    html: "",
    title: "",
    keyword: "",
    description: "",
    tags: [],
  });

  latestStateRef.current = state;

  useEffect(function () {
    if (!panelRef.current || !window.SeoEditorPluginIframe) return;

    pluginRef.current = window.SeoEditorPluginIframe.mountSeoPlugin({
      target: panelRef.current,
      config: {
        // Bắt buộc của plugin: source để plugin đọc/ghi dữ liệu.
        source: {
          html: {
            get: function () {
              return latestStateRef.current.html;
            },
          },
          title: {
            get: function () {
              return latestStateRef.current.title;
            },
            set: function (value) {
              // Logic hệ thống tích hợp tự viết: cập nhật state React.
              setState(function (current) {
                return { ...current, title: value || "" };
              });
            },
          },
          keyword: {
            get: function () {
              return latestStateRef.current.keyword;
            },
            set: function (value) {
              setState(function (current) {
                return { ...current, keyword: value || "" };
              });
            },
          },
          description: {
            get: function () {
              return latestStateRef.current.description;
            },
            set: function (value) {
              setState(function (current) {
                return { ...current, description: value || "" };
              });
            },
          },
          tags: {
            get: function () {
              return latestStateRef.current.tags;
            },
            set: function (value) {
              setState(function (current) {
                return { ...current, tags: value || [] };
              });
            },
          },
        },
      },
    });

    return function () {
      if (pluginRef.current) {
        pluginRef.current.destroy();
        pluginRef.current = null;
      }
    };
  }, []);

  useEffect(function () {
    if (pluginRef.current) {
      // Logic hệ thống tích hợp tự viết: state đổi thì báo plugin đọc lại.
      pluginRef.current.refresh();
    }
  }, [state]);

  return <div ref={panelRef} />;
}
```

## Ví Dụ Dữ Liệu Nằm Trong DOM Input

Nếu dữ liệu của hệ thống tích hợp đang nằm trong DOM input, `set` có thể cập nhật trực tiếp input đó.

```js
keyword: {
  get: function () {
    return keywordInput.value;
  },
  set: function (value) {
    keywordInput.value = value || "";
    notifySeoInputChanged();
  },
}
```

`notifySeoInputChanged()` không phải hàm của plugin. Đây là logic hệ thống tích hợp tự viết.

Ví dụ hàm này có thể làm các việc như:

- Cập nhật state của màn hình.
- Lưu giá trị mới vào model của bài viết.
- Gọi `instance.refresh()` để plugin tính lại điểm.

```js
function notifySeoInputChanged() {
  instance.refresh();
}
```

## Chế Độ Inline Và Popup

Render vào một panel có sẵn:

```js
window.SeoEditorPluginIframe.mountSeoPlugin({
  target: document.querySelector("#seo-panel"),
  config: { source: source },
});
```

Để plugin tự tạo popup:

```js
window.SeoEditorPluginIframe.mountSeoPlugin({
  config: { source: source },
});
```

## Xử Lý Sự Cố

### `window.SeoEditorPluginIframe` bị undefined

Kiểm tra:

- File script đã tải thành công chưa.
- Đường dẫn script có đúng không.
- Code gọi `mountSeoPlugin` sau khi script đã tải chưa.

### Điểm SEO không cập nhật khi sửa nội dung editor

Nguyên nhân thường gặp: hệ thống tích hợp chưa gọi `refresh()`.

Sửa:

```js
editor.model.document.on("change:data", function () {
  instance.refresh();
});
```

### Bấm áp dụng gợi ý nhưng dữ liệu bên ngoài không đổi

Nguyên nhân thường gặp: field thiếu `set`.

Sai:

```js
keyword: {
  get: function () {
    return seoData.keyword;
  },
}
```

Đúng:

```js
keyword: {
  get: function () {
    return seoData.keyword;
  },
  set: function (value) {
    seoData.keyword = value || "";
  },
}
```

### Gợi ý hiển thị đúng nhưng áp dụng sai giá trị

Plugin áp dụng `key`, không áp dụng `label`.

```js
{ key: "giá trị để lưu", label: "text để hiển thị" }
```

### Tags không hiển thị đúng

Dùng một trong các format sau:

```js
{ key: 101, label: "SEO nội dung" }
```

hoặc:

```js
{ Id: 101, Name: "SEO nội dung" }
```

### Nút đồng bộ alt ảnh không hoạt động

Cần khai báo:

```js
syncCorrectImageAlt: {
  set: function (html) {
    editor.setData(html || "");
  },
}
```

### Plugin hiển thị nhưng một số field bị ẩn

Kiểm tra `config.ui.visibility`.

```js
ui: {
  visibility: {
    keyword: true,
    title: true,
    description: true,
    tags: true,
  },
}
```

## Checklist Tích Hợp

Trước khi coi là tích hợp xong, kiểm tra:

- Đã tải script plugin.
- `window.SeoEditorPluginIframe.mountSeoPlugin` tồn tại.
- `config.source.html.get()` trả về HTML hiện tại của bài viết.
- `title`, `keyword`, `description` trả về giá trị hiện tại.
- Các field cần áp dụng gợi ý có `set(value)`.
- Hệ thống tích hợp gọi `instance.refresh()` khi nội dung editor thay đổi.
- Hệ thống tích hợp gọi `instance.refresh()` khi form SEO bên ngoài plugin thay đổi.
- `getFieldSuggestions` trả về `{ key, label }[]` nếu bật gợi ý AI.
- `searchTags` trả về `{ key, label }[]` nếu bật tìm kiếm tags.
- `source.tags.get/set` tồn tại nếu bật UI tags.
- `source.syncCorrectImageAlt.set` tồn tại nếu bật đồng bộ alt ảnh.
- `config.domain` được truyền nếu cần check link nội bộ/link ngoài chính xác.
- `instance.destroy()` được gọi khi màn hình tích hợp unmount.

## Mẫu Ngắn Nhất Để Copy <button type="button" title="Sao chép mẫu ngắn nhất" aria-label="Sao chép mẫu ngắn nhất" onclick="navigator.clipboard.writeText(this.parentElement.nextElementSibling.innerText.trim())" style="border:0;background:transparent;color:#344054;cursor:pointer;padding:0 4px;vertical-align:middle;"><svg width="15" height="15" viewBox="0 0 24 24" fill="none" aria-hidden="true" style="vertical-align:text-bottom;"><rect x="9" y="9" width="10" height="10" rx="2" stroke="currentColor" stroke-width="2"></rect><path d="M5 15H4C2.9 15 2 14.1 2 13V4C2 2.9 2.9 2 4 2H13C14.1 2 15 2.9 15 4V5" stroke="currentColor" stroke-width="2" stroke-linecap="round"></path></svg></button>

```js
var source = {
  html: {
    get: function () {
      return editor.getData();
    },
  },
  title: {
    get: function () {
      return seoData.title;
    },
    set: function (value) {
      seoData.title = value || "";
    },
  },
  keyword: {
    get: function () {
      return seoData.keyword;
    },
    set: function (value) {
      seoData.keyword = value || "";
    },
  },
  description: {
    get: function () {
      return seoData.description;
    },
    set: function (value) {
      seoData.description = value || "";
    },
  },
};

var instance = window.SeoEditorPluginIframe.mountSeoPlugin({
  target: document.querySelector("#seo-panel"),
  config: {
    source: source,
    api: {
      getFieldSuggestions: getFieldSuggestions,
      searchTags: searchTags,
    },
  },
});

editor.model.document.on("change:data", function () {
  instance.refresh();
});
```

Một câu cần nhớ:

```txt
Plugin đọc bằng get(), ghi bằng set(value), và hệ thống tích hợp gọi refresh() khi dữ liệu thay đổi.
```
