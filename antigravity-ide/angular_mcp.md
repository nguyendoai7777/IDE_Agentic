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

### A. Quy tắc TypeScript
* **Strict Mode:** Luôn bật chế độ kiểm tra kiểu nghiêm ngặt (`strict: true` trong `tsconfig.json`).
* **Type Inference:** Ưu tiên sử dụng cơ chế tự suy luận kiểu của TypeScript khi kiểu dữ liệu đã quá rõ ràng.
* **Tránh kiểu `any`:** Tuyệt đối không sử dụng kiểu dữ liệu `any`. Thay vào đó, hãy sử dụng `unknown` nếu kiểu dữ liệu chưa được xác định chắc chắn.

### B. Quy tắc Component & Template
* **Standalone Components:** 
  * Luôn sử dụng Standalone Components thay thế cho `NgModules`.
  * **Lưu ý quan trọng:** Không khai báo thuộc tính `standalone: true` bên trong decorator `@Component` vì đây là giá trị mặc định từ phiên bản Angular v20+.
* **Ràng buộc Host:** Không sử dụng decorator `@HostBinding` và `@HostListener`. Thay vào đó, khai báo trực tiếp trong thuộc tính `host` của decorator `@Component` hoặc `@Directive`.
* **Hình ảnh (Images):** Sử dụng directive `NgOptimizedImage` (`rawSrc`) cho tất cả các hình ảnh tĩnh. (Lưu ý: không dùng cho ảnh dạng inline base64).
* **Đơn giản hóa Template:** Tránh viết các logic phức tạp trong template. Sử dụng cú pháp Control Flow thế hệ mới (`@if`, `@for`, `@switch`) thay cho các directive cũ (`*ngIf`, `*ngFor`, `*ngSwitch`).
* **Đường dẫn (Paths):** Khi sử dụng template hoặc style file bên ngoài, hãy khai báo đường dẫn tương đối (relative path) đến file component `.ts`.
* **CSS Framework:** Sử dụng Tailwind CSS v4.

### C. Quản lý trạng thái (State Management) & Signals
* **Signals:** Sử dụng Signals để quản lý trạng thái cục bộ của component.
* **Derived State:** Sử dụng hàm `computed()` để định nghĩa các trạng thái phái sinh.
* **Signal Updates:** Luôn giữ việc chuyển đổi trạng thái thuần khiết (pure & predictable). Không dùng phương thức `mutate()` trên signal, hãy thay thế bằng `update()` hoặc `set()`.
* **Inputs & Outputs:** Sử dụng các hàm Reactive Input/Output mới: `input()`, `input.required()`, và `output()` thay vì các decorator cũ `@Input()` và `@Output()`.

### D. Service & Forms
* **Services:**
  * Mỗi service chỉ đảm nhận một trách nhiệm duy nhất (Single Responsibility).
  * Sử dụng decorator `@Service()` thay thế cho `@Injectable()`.
  * Sử dụng hàm `inject()` để tiêm phụ thuộc thay vì khai báo trong Constructor.
* **Forms:**
  * Không sử dụng Template-driven Forms.
  * Ưu tiên sử dụng Signal Forms (`@angular/forms/signals`) thay vì Reactive Forms truyền thống.

---

## 3. Các công cụ Angular MCP khả dụng trong IDE

Khi đã cấu hình thành công, AI Agent có thể truy cập các công cụ hỗ trợ Angular dưới đây:

1. **`angular-cli/list_projects`**: Quét toàn bộ cấu trúc dự án (apps, libraries, targets).
2. **`angular-cli/get_best_practices`**: Đọc các coding standards được cấu hình riêng cho dự án.
3. **`angular-cli/search_documentation`**: Tra cứu nhanh tài liệu Angular chính thức.
4. **`angular-cli/onpush_zoneless_migration`**: Hỗ trợ chuyển đổi code sang chế độ Zoneless/OnPush.
