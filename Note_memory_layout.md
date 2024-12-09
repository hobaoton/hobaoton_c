# PHÂN VÙNG NHỚ TRONG 

## 1. Text segment

    - Chứa các lệnh thực thi (khi chạy cương trình program couter trỏ tới vùng nhớ text để thực hiện các dùng lệnh).

    - Chỉ cho phép đọc và thực thi, không cấp quyền ghi.

    
## 2. Data segment (dữ liệu đã được khưởi tạo)

    - Chứa biến: toàn cục, static toàn cục, static cục bộ. Và con trỏ toàn cục, con trỏ tĩnh địa chỉ được cấp phát ở phân vùng data với giá trị khởi tọa khác không.

    - Các biến được khởi tạo với giá trị khác không.

    - Các biến được lưu trữ trên phân vùng Data được phép đọc, và thay đổi giá trị của biến (cho phép truy cập vào địa chỉ của biến để thay đổi giá trị).

    -Tất cả các biến sẽ được thu hồi khi chương trình kết thúc.


    EX1:
    \`\`\`c
#include <stdio.h>

int a = 10; // biến toàn cục khác 0 -> data

static int b = 20; // biến static toàn cục khác 0 -> data

void *ptr = &b; // con trỏ toàn cục khác 0 -> data

void function() {
    static int staticLocalVar = 30; // biến static cục bộ khác không -> data
}

int main() {
    
    return 0;
} \`\`\`

    - Đối với các biến const (const int a=0) hay con trỏ char (char *str = "hello world") thì đều được lưu ở phân vùng data, tuy nhiên chỉ được cấp quyền truy cập là đọc (read only).(đối với toàn cục)

## 3. BSS segment (dữ liệu chưa khởi tạo)

    - Chứa các con trỏ toàn cục và con trỏ static toàn cục hoặc cục bộ chưa được khởi tạo hoặc được khởi tạo với giá trị 0.

    - Chứa các biến toàn cục và biến static toàn cục và cục bộ chưa được khởi tạo hoặc được khởi tạo với giá trị 0.

    - Quyền truy cập bao gồm đọc và thay đổi giá trị dữ liệu

    - Tất cả các biến được thu hồi khi kết thúc chương trình.

    -> Vai trò giống Data chỉ khác nhau giá trị khởi tạo là chưa khởi tạo hoặc bằng không.

    EX2:
\`\`\`c
#include <stdio.h>

typedef struct{
    int a;
    int b;
} Point_Data;

Point_Data a1 = {5, 6}; // biến a1 có giá trị khác 0 -> Data
Point_Data a2 = {0, 0}; // biến a2 có giá trị bằng 0 -> BSS
Point_Data a3; // biến a3 chưa khởi tạo giá trị -> BSS

int x = 0; // biến toàn cục = 0 -> BSS
int y = 5; // biến toàn cụccó giá trị khác 0 -> Data

static int m = 0; // biến static toàn cục = 0 -> BSS
static int n; // biến static chưa khởi tạo giá trị -> BSS
static int p = 5; // biến static có giá trị khác 0 -> Data

void function() {
    static int h = 30; // biến static cục bộ khác 0 -> data
    static int i = 0; // biến static cục bộ = 0 -> BSS
    static int k; // biến static cục bộ chưa khỏi tạo giá trị -> BSS
}

int main() {    
    return 0;
}\`\`\`

    Để kiểm tra các biến thuộc phân vừng nào thông qua file.s
    `gcc -E file.c -o file.i`
    `gcc -S file.i -o file.s`

## 4. Stack

    - Chứa các biến cục bộ và con trỏ cục bộ ( ngoại trừ static cục bộ), và các tham số truyền vào trong hàm. (lưu ý: ko phân biết giá trị)

    - Có thể đọc và thay đổi giá trị của biến trong suốt thời gian chạy chương trình.

    - Sau khi chạy xong hàm thì vị trí lưu trữ cac biến sẽ được thu hồi

    - Nguyên tắc: giá trị nào được cấp phát đầu tiên sẽ được thu hồi cuối cùng

    EX4:
#include <stdio.h>
int *ptr;
void test()
{
    int *ptr1;
    const int a = 9; // biến hằng nhưng được khai báo trong chương trình con nên được lưu ở stack và có thể đọc và ghi thông qua con trỏ toàn cục hoăc cục bộ
    ptr1 = &a;
    *ptr1 = 10;
    printf ("a = %d\n", a);
    int test = 0; // biến cục bộ -> stack
    test = 5; // biến cục bộ -> stack
    printf("test: %d\n",test);
}
int sum(int a, int b)
{
    int c = a + b; // biến cục bộ -> stack
    printf("sum: %d\n",c);
    return c; // biến cục bộ -> stack
}
int main() {

    sum(3,5);
    test();       
    return 0;
}

## 5. Heap
    -Địa chỉ phân vùng heap được dùng lưu trữ giá trị cấp phát động. Gồm các hàm như: malloc, calloc, realloc
    -Giá trị tại địa chỉ phan vưng heap ko tự thu hồi khi kết thúc chương trình mà được thu hồi qua hàm 
    Free(tên mảng động thu hồi)