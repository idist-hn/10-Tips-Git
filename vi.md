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

##5. Never Lose Track of a Commit

Let’s say you committed something you didn’t want to and ended up doing a hard reset to come back to your previous state. Later, you realize you lost some other information in the process and want to get it back, or at least view it. This is where ```git reflog``` can help.

A simple ```git log``` shows you the latest commit, its parent, its parent’s parent, and so on. However, ```git reflog``` is a list of commits that the head was pointed to. Remember that it’s local to your system; it’s not a part of your repository and not included in pushes or merges.

If I run ```git log```, I get the commits that are a part of my repository:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946446git-ninja-04.png)

However, a ```git reflog``` shows a commit (```b1b0ee9 – HEAD@{4}```) that was lost when I did a hard reset:

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946447git-ninja-05.png)

##6. Staging Parts of a Changed File for a Commit

It is generally a good practice to make feature-based commits, that is, each commit must represent a feature or a bug fix. Consider what would happen if you fixed two bugs, or added multiple features without committing the changes. In such a situation situation, you could put the changes in a single commit. But there is a better way: Stage the files individually and commit them separately.

Let’s say you’ve made multiple changes to a single file and want them to appear in separate commits. In that case, we add files by prefixing ```-p``` to our add commands.

```

git add -p [file_name]

```

Let’s try to demonstrate the same. I have added three new lines to ```file_name``` and I want only the first and third lines to appear in my commit. Let’s see what a ```git diff``` shows us.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946449git-ninja-06.png)

And let’s see what happes when we prefix a ```-p``` to our ```add``` command.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946450git-ninja-07.png)

It seems that Git assumed that all the changes were a part of the same idea, thereby grouping it into a single hunk. You have the following options:

* Enter y to stage that hunk
* Enter n to not stage that hunk
* Enter e to manually edit the hunk
* Enter d to exit or go to the next file.
* Enter s to split the hunk.

In our case, we definitely want to split it into smaller parts to selectively add some and ignore the rest.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946452git-ninja-08.png)

As you can see, we have added the first and third lines and ignored the second. You can then view the status of the repository and make a commit.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946454git-ninja-09.png)

##7. Squash Multiple Commits

When you submit your code for review and create a pull request (which happens often in open source projects), you might be asked to make a change to your code before it’s accepted. You make the change, only to be asked to change it yet again in the next review. Before you know it, you have a few extra commits. Ideally, you could squash them into one using the ```rebase``` command.

```

git rebase -i HEAD~[number_of_commits]

```

If you want to squash the last two commits, the command that you run is the following.

```

git rebase -i HEAD~2

```

On running this command, you are taken to an interactive interface listing the commits and asking you which ones to squash. Ideally, you ```pick``` the latest commit and ```squash``` the old ones.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946455git-ninja-10.png)

You are then asked to provide a commit message to the new commit. This process essentially re-writes your commit history.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946457git-ninja-11.png)

##8. Stash Uncommitted Changes

Let’s say you are working on a certain bug or a feature, and you are suddenly asked to demonstrate your work. Your current work is not complete enough to be committed, and you can’t give a demonstration at this stage (without reverting the changes). In such a situation, ```git stash``` comes to the rescue. Stash essentially takes all your changes and stores them for further use. To stash your changes, you simply run the following-

```

git stash

```

To check the list of stashes, you can run the following:

```

git stash list

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946458git-ninja-12.png)

If you want to un-stash and recover the uncommitted changes, you apply the stash:

```

git stash apply

```

In the last screenshot, you can see that each stash has an indentifier, a unique number (although we have only one stash in this case). In case you want to apply only selective stashes, you add the specific identifier to the ```apply``` command:

```

git stash apply stash@{2}

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946461git-ninja-13.png)


##9. Check for Lost Commits

Although ```reflog``` is one way of checking for lost commits, it’s not feasible in large repositories. That is when the ```fsck``` (file system check) command comes into play.

```

git fsck --lost-found

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946463git-ninja-14.png)

Here you can see a lost commit. You can check the changes in the commit by running ```git show [commit_hash]``` or recover it by running ```git merge [commit_hash]```.

```git fsck``` has an advantage over ```reflog```. Let’s say you deleted a remote branch and then cloned the repository. With ```fsck``` you can search for and recover the deleted remote branch.


##10. Cherry Pick

I have saved the most elegant Git command for the last. The ```cherry-pick``` command is by far my favorite Git command, because of its literal meaning as well as its utility!

In the simplest of terms, ```cherry-pick``` is picking a single commit from a different branch and merging it with your current one. If you are working in a parallel fashion on two or more branches, you might notice a bug that is present in all branches. If you solve it in one, you can cherry pick the commit into the other branches, without messing with other files or commits.

Let’s consider a scenario where we can apply this. I have two branches and I want to ```cherry-pick``` the commit ```b20fd14: Cleaned junk``` into another one.

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946465git-ninja-15.png)

I switch to the branch into which I want to ```cherry-pick``` the commit, and run the following:

```

git cherry-pick [commit_hash]

```

![ahihi](https://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2014/06/1402946467git-ninja-16.png)

Although we had a clean ```cherry-pick``` this time, you should know that this command can often lead to conflicts, so use it with care.

##Kết luận

Với cái này, chúng tôi đã hoàn tất danh sách các mẹo mà tôi nghĩ có thể giúp bạn đưa kỹ năng của mình lên 1 tầm cao mới. Git là 1 công cụ tốt nhất hiện nay và nó có thể hoàn thành bất cứ thứ gì bạn có thể tưởng tượng. Vì vậy, hãy luôn cố gắng để thử thách bản thân mình với Git. Rất có thể, bạn sẽ hoàn thiện được việc học một vài thứ mới mẻ!
