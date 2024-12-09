# Phân biệt `malloc`, `calloc`, và `realloc` trong C

## 1. `malloc`
- **Công dụng**: Cấp phát bộ nhớ động.
- **Cú pháp**: `void* malloc(size_t size);`
- **Khởi tạo**: Không khởi tạo bộ nhớ được cấp phát, bộ nhớ có thể chứa giá trị rác.
- **Ví dụ**:
  ```c
  int *arr = (int*)malloc(5 * sizeof(int));


## 2. `calloc`

- **Công dụng**:Cấp phát bộ nhớ động và khởi tạo tất cả các phần tử về 0.

- **Cú pháp**:`void* calloc(size_t num, size_t size);`

- **Khởi tạo**: Khởi tạo tất cả các phần tử về 0.

- **Ví dụ**:
\`\`\`c
#include <stdio.h>
#include <stdlib.h>
int main() {
    int *arr = (int*)calloc(5, sizeof(int));

    if (arr == NULL) {
        printf("Không thể cấp phát bộ nhớ.\n");
        return 1;
    }

    for (int i = 0; i < 5; i++) {
        printf("arr[%d] = %d\n", i, arr[i]);
    }

    free(arr);
    return 0;
}\`\`\`

## 3. `realloc`

- **Công dụng**: Thay đổi kích thước của bộ nhớ đã được cấp phát trước đó.

- **Cú pháp**: `void* realloc(void* ptr, size_t size);`

- **Khởi tạo**: Giữ nguyên dữ liệu hiện có, phần bộ nhớ mới (nếu có) không được khởi tạo.

- **Ví dụ**
  `arr = (int*)realloc(arr, 10 * sizeof(int));`

**Đặc điểm**	                      **malloc**	               **calloc**	             **realloc**
**Cấp phát bộ nhớ:**	                Động                       Động	                   Động
**Khởi tạo giá trị:**	                Không	                     Có (tất cả về 0)	       Không
**Thay đổi kích thước:**              Không	                     Không	                 Có
**Giải phóng bộ nhớ:**                Cần free	                 Cần free	               Cần free
