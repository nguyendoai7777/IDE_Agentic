# Hướng dẫn cấu hình Angular MCP cho Antigravity IDE & Quy tắc code (Code Rules)

Tài liệu này hướng dẫn cách cấu hình Angular Model Context Protocol (MCP) trên các hệ điều hành khác nhau cho **Antigravity IDE**, cũng như các quy tắc phát triển ứng dụng (Code Rules/Conventions) dựa trên cấu hình dự án của bạn.

---

## 1. Cấu hình Angular MCP cho Antigravity IDE

Để Antigravity IDE nhận diện được các công cụ của Angular MCP, bạn cần cấu hình tệp cấu hình trung tâm `mcp_config.json` tương ứng với hệ điều hành đang sử dụng.

### Đường dẫn file cấu hình theo Hệ điều hành:

| Hệ điều hành | Đường dẫn tệp cấu hình (`mcp_config.json`) |
| :--- | :--- |
| **Linux (Ubuntu)** | `~/.gemini/antigravity-ide/mcp_config.json` <br> (Chi tiết: `/home/<username>/.gemini/antigravity-ide/mcp_config.json`) |
| **macOS** | `~/.gemini/antigravity-ide/mcp_config.json` <br> (Chi tiết: `/Users/<username>/.gemini/antigravity-ide/mcp_config.json`) |
| **Windows** | `%USERPROFILE%\.gemini\antigravity-ide\mcp_config.json` <br> (Chi tiết: `C:\Users\<username>\.gemini\antigravity-ide\mcp_config.json`) |

### Nội dung cấu hình mẫu:

Tạo hoặc chỉnh sửa tệp `mcp_config.json` theo đường dẫn trên với nội dung sau:

```json
{
  "mcpServers": {
    "angular-cli": {
      "command": "npx",
      "args": [
        "-y",
        "@angular/cli",
        "mcp"
      ]
    }
  }
}
```

> [!TIP]
> Nếu bạn đã cài đặt Angular CLI ở chế độ global (`npm install -g @angular/cli`) và lệnh `ng` khả dụng trong terminal hệ thống, bạn có thể tối ưu hóa hiệu năng khởi động bằng cách dùng trực tiếp lệnh `ng`:
> ```json
> {
>   "mcpServers": {
>     "angular-cli": {
>       "command": "ng",
>       "args": ["mcp"]
>     }
>   }
> }
> ```

### Kích hoạt cấu hình:
Sau khi lưu tệp `mcp_config.json`, bạn cần tải lại môi trường làm việc của IDE:
* **VS Code / Cursor:** Mở Command Palette (`Ctrl + Shift + P` hoặc `Cmd + Shift + P`) -> chọn **`Developer: Reload Window`**.

---

## 2. Quy tắc phát triển và Quy chuẩn viết Code (Code Rules & Conventions)
* GEMINI model: đặt GEMINI.md tại root cho dễ nhất
* CLAUDE mode: ///     
Các quy chuẩn viết code dưới đây được định nghĩa nhằm đảm bảo hiệu năng, khả năng mở rộng, khả năng tiếp cận (Accessibility), và đồng bộ hóa với các phiên bản Angular hiện đại mới nhất (v20+).


## TypeScript Best Practices

- Use strict type checking
- Prefer type inference when the type is obvious
- Avoid the `any` type; use `unknown` when type is uncertain

## Angular Best Practices

- Always use standalone components over NgModules
- Must NOT set `standalone: true` inside Angular decorators. It's the default in Angular v20+.
- Use signals for state management
- Implement lazy loading for feature routes
- Do NOT use the `@HostBinding` and `@HostListener` decorators. Put host bindings inside the `host` object of the `@Component` or `@Directive` decorator instead
- Use `NgOptimizedImage` for all static images.
  - `NgOptimizedImage` does not work for inline base64 images.

## Accessibility Requirements

- It MUST pass all AXE checks.
- It MUST follow all WCAG AA minimums, including focus management, color contrast, and ARIA attributes.

### Components

- Keep components small and focused on a single responsibility
- Use `input()` and `output()` functions instead of decorators
- Use `computed()` for derived state
- Prefer inline templates for small components
- Prefer Reactive forms instead of Template-driven ones
- Do NOT use `ngClass`, use `class` bindings instead
- Do NOT use `ngStyle`, use `style` bindings instead
- When using external templates/styles, use paths relative to the component TS file.
- use tailwindcss 4

## State Management

- Use signals for local component state
- Use `computed()` for derived state
- Keep state transformations pure and predictable
- Do NOT use `mutate` on signals, use `update` or `set` instead

## Templates

- Keep templates simple and avoid complex logic
- Use native control flow (`@if`, `@for`, `@switch`) instead of `*ngIf`, `*ngFor`, `*ngSwitch`
- Use the async pipe to handle observables
- Do not assume globals like (`new Date()`) are available.

## Services

- Design services around a single responsibility
- Use @Service() decorator instead of @Injectable()
- Use the `inject()` function instead of constructor injection

## Forms

- never use Template Drivent Form
- prefer use signal form (@angular/forms/signals) instead of ReactiveForms
