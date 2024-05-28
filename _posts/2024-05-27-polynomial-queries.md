---
layout: post
title: Truy vấn cập nhật bậc thang - Polynomial Queries
subtitle: There's lots to learn!
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
mathjax: true
author: Bill Smith
---

Chào mọi người! Hôm nay, mình muốn giới thiệu về một kỹ thuật rất phổ biến và hữu ích trong xử lí truy vấn trên cây Segment Tree, cũng như cây BIT (Binary Indexed Tree) đó là cập nhật bậc thang (hay truy vấn bậc thang). Để hiểu sâu hơn về kỹ thuật này, trước hết, bạn cần phải có kiến thức về Lazy Propagation (hay còn gọi là Lazy Update) cùng với cơ chế hoạt động của cây BIT.

# Bài toán 1
Cho mảng mảng có $$n$$ phần tử gồm các số nguyên. Cho $$q$$ truy vấn, mỗi truy vấn có dạng:
- $$u$$ $$v$$: Tăng $$a[u]$$ lên 1 đơn vi, $$a[u + 1]$$ lên 2 đơn vị, ... hay nói tổng quát là tăng $$i$$ trong đoạn $$[l, r]$$ lên $$i - l + 1$$ đơn vị.

Ý tưởng: Nếu bài toán này chỉ tăng các số trong đoạn $$[u, v]$$ lên một hằng số cụ thể thì rất đơn giản. Chúng ta chỉ việc dùng một mảng hiệu $$b$$. Với một truy vấn $$[u, v]$$ ta chỉ cần gán $$b[u]$$ += $$val$$, $$b[v + 1]$$ -= $$val$$ và sau đó ta chỉ cần tổng dồn lại bằng cách $$b[i]$$ += $$b[i - 1]$$ là xong.

Nhận xét: Với mỗi phần tử nằm trong đoạn $$[u, v]$$, ta có tách thành 2 truy vấn nhỏ hơn:
- $$1$$. Tăng các phần tử trong đoạn $$[u, v]$$ lên một lượng không đổi là $$(1 - u)$$.
- $$2$$. Vỡi mỗi $$i$$ thuộc $$[u, v]$$, ta tăng phần tử $$a[i]$$ lên $$i$$ đơn vị

Với truy vấn 1, ta nhận ra đó là truy vấn tăng một lượng không đổi cơ bản mình đã nó ở trên. Nên vấn để bây giờ là làm sao để giải quyết truy vấn 2.

Với truy vấn 2, thay vì xét từng giá trị $i$ thuộc đoạn $$[u, v]$$ và tăng lượng cố định là $$i$$ thì ta chỉ cần đếm số lần phần tử $i$ được tăng lên qua mỗi truy vấn và kết quả chỉ cần lấy $$i$$ $$*$$ $$cnt[i]$$.

Độ phức tạp cơ bản nó chỉ là $$O(n)$$.

# Bài toán 2
Cho mảng mảng có $$n$$ phần tử gồm các số nguyên. Cho $$q$$ truy vấn, mỗi truy vấn có dạng:
- $$1$$ $$u$$ $$v$$: Tăng $$a[u]$$ lên 1 đơn vi, $$a[u + 1]$$ lên 2 đơn vị, ... hay nói tổng quát là tăng $$i$$ trong đoạn $$[l, r]$$ lên $$i - l + 1$$ đơn vị.
- $$2$$ $$k$$: In ra giá trị $$a[k]$$.

Cơ bản bài toán này rất giống bài toán 1 ở trên, khác ở chỗ nó có thêm truy vấn in ra giá trị thứ $$k$$. Thì đơn giản mình cũng tính như trên, ta sẽ duy trì hai giá trị $$b[i]$$ và $$cnt[i]$$ bằng các thực hiện hai truy vấn con mình đã nói ở trên đồng thời tính thêm giá trị $$b[i]$$ + $$i$$ $$*$$ $$cnt[i]$$. 

Cách đơn giản có thể sử dụng cây BIT, ta lưu hai cây BIT, một cây để quản lí giá trị $$b$$, cây còn lại quản lí giá trị $$cnt$$.

Ngoài ra, bạn có thể sử dụng Segment tree, nhưng sẽ dài và phức tạp hơn so với cây BIT.

Độ phức tạp: $$O(nlogn)$$.

Code tham khảo của mình: https://ideone.com/jWoguW

# Bài toán 3
Cho mảng mảng có $$n$$ phần tử gồm các số nguyên. Cho $$q$$ truy vấn, mỗi truy vấn có dạng:
- $$1$$ $$u$$ $$v$$: Tăng $$a[u]$$ lên 1 đơn vi, $$a[u + 1]$$ lên 2 đơn vị, ... hay nói tổng quát là tăng $$i$$ trong đoạn $$[l, r]$$ lên $$i - l + 1$$ đơn vị.
- $$2$$ $$u$$ $$v$$: Tính tổng các phần tử trong đoạn $$[u, v]$$.

Vì bài này có thao tác tính tổng nên ta phải dùng Lazy update để tính cập nhật giá trị của nút con là $$(1 - u)$$ $$*$$ $$(v - u + 1)$$ và $$cnt * sum(u, v)$$, trong đó $$sum(u, v)$$ là tính tổng các số liên tiếp từ $$u$$ đến $$v$$ được tính bằng công thức $$(l + r) * (r - l + 1) / 2$$. Trong đó ta cũng duy trì một cái Struct Lazy với ý nghĩa như mảng $$b$$ và $$cnt$$, mỗi lần sẽ cập nhật $$lazy[id].val$$ += $$(1 - u)$$ và $$lazy[id].cnt$$ += $$1$$.

Code tham khảo của mình: https://ideone.com/4ZMjQ0



