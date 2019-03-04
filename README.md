# cs500
cs500 design and analysis of algorithm

**Giải thuật sắp xếp Adaptive và Non-Adaptive**

Một giải thuật được xem như là adaptive, nếu nó tận dụng các phần tử đã được sắp xếp trong danh sách mà đã được sắp xếp. Đó là, trong khi sắp xếp nếu danh sách ban đầu có một số phần tử đã được sắp xếp, thì giải thuật dạng adaptive sẽ ghi nhận các phần tử này và sẽ cố gắng không thay đổi thứ tự của chúng.

Trái ngược với loại giải thuật trên, giải thuật dạng non-adaptive sẽ không ghi nhận các phần tử đã được sắp xếp trước đó. Giải thuật loại này sẽ vấn cố gắng sắp xếp lại từng phần tử trong danh sách ban đầu.

**Các khái niệm quan trọng trong giải thuật sắp xếp**

Dưới đây là phần giới thiệu ngắn gọn cho một số khái niệm xuất hiện trong khi thảo luận về các giải thuật sắp xếp:

`Thứ tự tăng`
Một dãy giá trị được xem như trong thứ tự tăng dần nếu phần tử đứng sau lớn hơn phần tử đứng trước. Ví dụ: 1, 3, 5, 6, 9.

`Thứ tự giảm`
Một dãy giá trị được xem như trong thứ tự giảm dần nếu phần tử đứng sau nhỏ hơn phần tử đứng trước. Ví dụ: 9, 6, 5, 3, 1.

`Thứ tự không tăng`
Một dãy giá trị được xem như trong thứ tự không tăng nếu phần tử đứng sau nhỏ hơn hoặc bằng phần tử đứng trước. Ví dụ: 9, 6, 5, 5, 1. Loại thứ tự này xuất hiện khi trong một dãy có chứa các giá trị giống nhau.

`Thứ tự không giảm`
Một dãy giá trị được xem như trong thứ tự không giảm nếu phần tử đứng sau lớn hơn hoặc bằng phần tử đứng trước. Ví dụ: 1, 5, 5, 6, 9. Loại thứ tự này xuất hiện khi trong một dãy có chứa các giá trị giống nhau.
