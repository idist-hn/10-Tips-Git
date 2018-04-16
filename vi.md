#10 mẹo nhỏ để giúp nâng cao kỹ năng sử dụng Git của bạn

Gần đây chúng tôi đã xuất bản một số bài hướng dẫn để giúp bạn làm quen với các khái niệm cơ bản của Git và sử dụng Git trong môi trường phát triển phần mềm. Các câu lệnh mà chúng ta đã đề cập trước đó đủ để giúp 1 lập trình viên tồn tại trong thế giới của Git. Trong bài viết này, chúng tôi sẽ cố gắng khám phá làm cách nào để quản lý thời gian của bạn 1 cách hiệu quả và tận dụng cách tính năng mà git đã cung cấp.

Ghi chú: Một số lệnh trong bài viết này chứa cả 1 phần của lệnh trong các dấu ngoặc vuông (ví dụ thêm -p [file_name]). Trong các ví dụ, bạn nên chèn thêm các số cần thiết, định danh, ví dụ như không cần có dấu ngoặc vuông.

##1. Tự động hoàn thành trong Git

Nếu bạn chạy những lệnh Git thông qua các dòng lệnh, đó là 1 nhiệm vụ thực sự mệt mỏi khi bạn phải gõ các lệnh bằng tay mỗi lần thực hiện. Để giúp cho việc này, bạn có thể bật tính năng tự động hoàn thành các câu lệnh Git chỉ trong 1 vài phút.

Để thực hiện việc này, thực hiện các thao tác sau trên hệ thống Unix:

```
cd ~
curl https://raw.github.com/git/git/master/contrib/completion/git-completion.bash -o ~/.git-completion.bash
```

Tiếp theo, thêm vào file ```~/.bash_profile``` các dòng lệnh sau:

```
if [ -f ~/.git-completion.bash ]; then
    . ~/.git-completion.bash
fi
```

Mặc dù tôi đã đề cập điều này trước đó, tôi vẫn chưa nhấn mạnh đủ về nó: Nếu bạn muốn sử dụng các tính năng của Git 1 cách đầy đủ, bạn chắc chắn phải chuyển sang giao diện dòng lệnh!

##2. Bỏ qua các tệp trong Git

Bạn có cảm thấy mệt khi phải biên dịch các file như (like ```.pyc```) xuất hiện trong các repository trên Git của bạn? Hoặc bạn có chán tới mức thêm luôn cả chúng vào Git? Đừng nhìn xa quá, đây là 1 trong những cách thông qua đó bạn có thể gọi Git để bỏ qua một số tập tin và cả các thư mục nữa. Đơn giản là chỉ cần tạo 1 file có tên ```.gitignore``` và liệt kê danh sách các file hoặc thư mục mà bạn không muốn Git theo dõi. Bạn có thể thêm các ngoại lệ bằng cách sử dụng dấu chấm than(!).

```
*.pyc
*.exe
my_db_config/

!main.pyc
```

##3. Ai đã sửa code của tôi?

Bản năng tự nhiên của con người là đổ lỗi cho người khác khi gặp điều gì đó bất ổn. Nếu máy chủ sản xuất gặp sự cố, rất dễ dàng để tìm ra thủ phạm — chỉ cần thực hiện một ```git blame```. Câu lệnh này sẽ hiển thị cho bạn tác giả mỗi dòng trong file, commit có sự thay đổi cuối cùng trên dòng đó và thời gian của commit đó.

```
git blame [file_name]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946443git-ninja-01.png)

Và trong ảnh chụp màn hình dưới đây, bạn có thể thấy câu lệnh này sẽ thực hiện như thế nào trong 1 repository lớn hơn:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946441git-ninja-02.png)

##4. Xem lại lịch sử của Repository

Chúng tôi đã xem xét việc sử dụng ```git log``` trong bài hướng dẫn trước đó, tuy nhiên, ở đây là 3 option mà bạn nên biết.

* ```--oneline``` – Hiển thị các thông tin được nén lại bên cạnh mỗi commit để giảm bớt các commit hash và các thông điệp, tất cả được hiển thị trên 1 dòng.
* ```--graph``` – Option này rút ra và vẽ lại một biểu đồ dựa trên một lịch sử ở bên trái của đầu ra. Nó không được sử dụng nếu bạn đang muốn xem lịch sử của chỉ 1 nhánh.
* ```--all``` – Hiển thị tất cả lịch sử của tất cả các nhánh.

Dưới đây là những gì kết hợp được trong các tuỳ chọn như sau:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946444git-ninja-03.png)

##5. đừng bao giờ mất dấu một Commit

Giả sử bạn đã commit một vài thứ mà bạn không muốn và cuối cùng bạn phải thực hiện thiết lập lại tương đối khó khăn để trở về trạng thái trước đó. Một lúc sau, bạn nhận ra là bạn đánh mất một vài thông tin trong quá trình thực hiện và muốn lấy nó lại hoặc ít nhất là xem lại nó. Đây là nơi mà ```git reflog``` có thể giúp bạn.

Đơn giản là ```git log``` hiển thị cho bạn lần commit gần nhất, lần cha của nó, lần cha của cha nó và nhiều hơn thế. Tuy nhiên, ```git reflog``` lại là danh sách các commit mà người đứng đầu đã chỉ ra. Hãy nhớ rằng nó chỉ tồn tại trên hệ thống của bạn; nó không phải là 1 phần của repository và nó không nằm trong những thao tác push hay merge.

Nếu tôi thực hiện lệnh ```git log```, Tôi sẽ nhận được các commit mà đó là 1 phần trong repository của tôi:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946446git-ninja-04.png)

Tuy nhiên , lệnh ```git reflog``` hiển thị một commit (```b1b0ee9 – HEAD@{4}```) mà đã bị mất khi tôi thực hiện việc hard reset:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946447git-ninja-05.png)

##6. Phân loại các phần của một file đã được thay đổi cho một Commit

Nói chung đây là một ví dụ tốt để thực hiện các commit dựa trên các tính năng, đó là, mỗi commit phải đại diện cho 1 tính hoặc hoặc 1 sửa lỗi nào đó. Hãy xem xét điều gì sẽ xảy ra nếu bạn sửa 2 lỗi, hoặc thêm nhiều tính năng mà không commit những thay đổi đó.Trong tình huống như vậy, bạn có thể đặt những thay đổi vào chỉ một commit. Nhưng đây mới là cách tốt nhất: Sắp xếp các file riêng lẻ, commit chúng một cách riêng biệt.

Giả sử bạn đã thực hiện nhiều thay đổi trên 1 file duy nhất và muốn chúng xuất hiện ở các commit riêng biệt.Trong trường hợp này, chúng tôi thêm các file bằng các tiền tố ```-p``` vào các câu lệnh của chúng tôi.

```

git add -p [file_name]

```
Hãy cùng chứng minh điều tương tự. Tôi có thêm 3 dòng mới vào ```file_name``` và tôi muốn chỉ dòng đầu và dòng 3 xuất hiện trong commit của tôi. Hãy xem ```git_diff``` hiển thị cho ta những gì.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946449git-ninja-06.png)

và hãy xem điều gì xảy ra khi chúng tôi thêm tiền tố  ```-p``` vào lệnh ```add``` của chúng tôi.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946450git-ninja-07.png)

Dường như Git cho rằng tất cả các thay đổi đều là 1 phần của cùng một ý tưởng, do đó nó nhóm lại thành 1 phần lớn.Bạn phải tuân theo các option sau:

* Nhập y để thực thi trên nhóm đó
* Nhập n để không thực thi trên nhóm đó
* Nhập e để thực hiện các thay đổi thủ công cho nhóm đó
* Nhập d để thoát hoặc đi tới file tiếp theo
* Nhập s để tách nhóm đó ra.

Trong trường hợp của chúng tôi, chúng tôi chắc chắn là muốn phân tách thành các phần nhỏ hơn để chọn lọc một số thay đổi và bỏ qua các phần còn lại.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946452git-ninja-08.png)

Bạn có thể thấy, chúng tôi đã thêm dòng đầu và dòng 3, bỏ qua dòng thứ 2.Bạn có thể tiếp tục xem trạng thái của repository và tạo các commit. 

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946454git-ninja-09.png)

##7. Tập hợp nhiều Commits

Khi bạn submit code của bạn để xem xét và tạo pull request (điều thường thấy ở các dự án mã nguồn mở),  bạn có thể được yêu cầu thay đổi các mã của mình trước khi nó được chấp nhận. Bạn tạo ra các thay đổi, chỉ khi được yêu cầu thay đổi nó một lần nữa trong lần xem xét tiếp theo. Trước khi bạn biết nó, bạn có một vài commit bổ sung. Ý tưởng là, bạn có thể tập hợp chúng thành một sử dụng lệnh ```rebase```.

```

git rebase -i HEAD~[number_of_commits]

```

Nếu bạn muốn tập hợp 2 commit gần nhất, câu lệnh mà bạn chạy sẽ như này.

```

git rebase -i HEAD~2

```

Khi chạy lệnh này, bạn được chuyển tới giao diện tương tác với danh sách các commit và hỏi bạn những commi nào bạn muốn hợp lại. Ý tưởng là bạn ```pick``` commit cuối cùng và  ```squash``` với 1 cái cũ hơn.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946455git-ninja-10.png)

Sau đó bạn sẽ được yêu cầu cung cấp một message cho commit mới. Bản chất của tiến trình này là ghi đè lên lịch sử commit của bạn.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946457git-ninja-11.png)

##8. Giấu những thay đổi không được commit

Giả sử bạn đang làm việc trên 1 lỗi hay 1 tính năng nhất định, và bạn đột nhiên được yêu cầu phải giải trình công việc của bạn. Công việc hiện tại của bạn chưa hoàn thiện đủ để commit, và bạn không thể giải trình tại giai đoạn này  (mà không cần hoàn nguyên các thay đổi). Trong trường hợp như vậy, ```git stash``` đến để giải cứu vấn đề này. Về cơ bản việc này lấy tất cả những thay đổi của bạn và lưu trữ chúng để sử dụng tiếp. Để giấu các thay đổi của bạn, đơn giản bạn chỉ cần chạy như sau-

```

git stash

```

Để kiểm tra danh sách được giấu đi, bạn có thể chạy lệnh:

```

git stash list

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946458git-ninja-12.png)

Nếu bạn không muốn giấu nữa và phục hồi các thay đổi chưa được commit, bạn áp dụng lệnh:

```

git stash apply

```

In the last screenshot, you can see that each stash has an indentifier, a unique number (although we have only one stash in this case). In case you want to apply only selective stashes, you add the specific identifier to the ```apply``` command:

```

git stash apply stash@{2}

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946461git-ninja-13.png)


##9. Kiểm tra các Commit bị mất

Mặc dù ```reflog``` là một cách để kiểm tra các commit bị mất, nhưng nó lại không khả thi trong các repository lớn. Đây là khi mà lệnh ```fsck``` (kiểm tra hệ thống tập tin) vào cuộc chơi.

```

git fsck --lost-found

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946463git-ninja-14.png)

Ở đây bạn có thể thấy commit bị mất. Bạn có thể kiểm tra những thay đổi trong commit bằng cách chạy lệnh ```git show [commit_hash]``` hoặc phục hồi chúng bằng lệnh ```git merge [commit_hash]```.

Lệnh ```git fsck``` có nhiều lợi thế hơn lệnh ```reflog```. Giả sử bạn xoá mất một nhánh remote và sau đó clone lại repository. với lệnh ```fsck``` bạn có thể tìm kiếm và phục hồi nhánh từ xa mà bạn đã xoá.


##10. Lấp liếm bằng chứng (Lỗi suy luận về bằng chứng không đầy đủ)

Tôi đã lưu lại lệnh Git nguyên vẹn nhất cho lần cuối cùng. Và lệnh ```cherry-pick``` là lệnh mà tôi yêu thích nhất, bởi vì cả nghĩa đen cũng như nghĩa bóng đều tốt như lợi ích nó mang lại!
Trong các điều khoản đơn giản nhất, lệnh ```cherry-pick``` sẽ lấy một commit từ 1 nhánh khác và merge nó với nhánh hiện tại của bạn. Nếu bạn đang làm việc theo kiểu song song trên 2 hay nhiều nhánh 1 lúc, bạn có thể nhận thấy 1 lỗi tồn tại trên tất cả các nhánh. Nếu bạn giải quyết nó trên 1 nhánh, bạn cần lấp liếm nó vào các nhánh khác, mà không gặp rắc rối với các file khắc hoặc commit khác.

Hãy xem xét 1 kịch bản mà chúng ta có thể áp dụng điều này. Tôi có 2 nhanánh và tôi muốn ```cherry-pick``` cái commit  ```b20fd14: Cleaned junk``` vào một nhánh kia.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946465git-ninja-15.png)

Tôi chuyển sang nhánh mà tôi muốn ```cherry-pick``` cái commit ấy và chạy lệnh:

```

git cherry-pick [commit_hash]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946467git-ninja-16.png)

Mặc dù chúng tôi đã dọn sạch ```cherry-pick``` vừa nãy, bạn nên biết rằng lệnh này thường có thể dẫn đến xung đột, do đó, hãy sử dụng nó 1 cách cẩn thận.

##Kết luận

Với cái này, chúng tôi đã hoàn tất danh sách các mẹo mà tôi nghĩ có thể giúp bạn đưa kỹ năng của mình lên 1 tầm cao mới. Git là 1 công cụ tốt nhất hiện nay và nó có thể hoàn thành bất cứ thứ gì bạn có thể tưởng tượng. Vì vậy, hãy luôn cố gắng để thử thách bản thân mình với Git. Rất có thể, bạn sẽ hoàn thiện được việc học một vài thứ mới mẻ!
