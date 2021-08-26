## Git và GitHub

| STT | CONTENT |
| ------ | ------ |
| 1 |Git|
| 2 |GitHub|
| 3 |Demo dự án thực tế|
 

# Git là gì?
![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/git_img_1.jpg)

Git là một hệ thống quản lý phiên bản phân tán (Distributed Version Control System – DVCS), nó là một trong những hệ thống quản lý phiên bản phân tán phổ biến nhất hiện nay. Git cung cấp cho mỗi lập trình viên kho lưu trữ (repository) riêng chứa toàn bộ lịch sử thay đổi.

## _Version Control System – VCS là gì?_
VCS là viết tắt của Version Control System là hệ thống kiểm soát các phiên bản phân tán mã nguồn mở. Các VCS sẽ lưu trữ tất cả các file trong toàn bộ dự án và ghi lại toàn bộ lịch sử thay đổi của file. Mỗi sự thay đổi được lưu lại và thành một version (phiên bản).

## _VCS có tác dụng như thế nào?_
1. Lưu lại lịch sử các version của bất kỳ thay đổi nào của dự án. Giúp xem lại các sự thay đổi hoặc khôi phục (revert) lại sau này.

2. Việc chia sẻ code trở nên dễ dàng hơn, lập trình viên có thể để public cho bất kỳ ai, hoặc private chỉ cho một số người có thẩm quyền có thể truy cập và lấy code về.

Vốn là một VCS nên Git cũng ghi nhớ lại toàn bộ lịch sử thay đổi của source code trong dự án. Lập trình sửa file, thêm dòng code tại đâu, xóa dòng code ở hàng nào…đều được Git ghi nhận và lưu trữ lại.

## _Git hoạt động như thế nào?_

Sự khác biệt chính giữa Git và bất kỳ VCS  khác (bao gồm Subversion…).

Về mặt khái niệm, hầu hết các hệ thống khác đều lưu trữ thông tin dưới dạng danh sách các thay đổi dựa trên file. Các hệ thống này (CVS, Subversion, Perforce, Bazaar, v.v.) coi thông tin chúng lưu giữ dưới dạng một tập hợp các file và những thay đổi được thực hiện đối với mỗi file theo thời gian.

![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/git_img_2.png)

Git không lưu trữ dữ liệu theo cách này. Thay vào đó, Git coi thông tin được lưu trữ là một tập hợp các snapshot – ảnh chụp toàn bộ nội dung tất cả các file tại thời điểm.
Mỗi khi bạn “commit”, Git sẽ “chụp” và tạo ra một snapshot cùng một tham chiếu tới snapshot đó. Để hiệu quả, nếu các tệp không thay đổi, Git sẽ không lưu trữ lại file — chỉ là một liên kết đến tệp giống file trước đó mà nó đã lưu trữ.

![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/git_img_3.png)

## _Mô Hình Hoạt Động_

![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/git_img_21.png)

Tất cả các thao tác thêm/sửa/xoá để thay đổi code sẽ được thực hiện trên working directory. Khi muốn lưu lại những thay đổi này, ta sẽ dùng câu lệnh git add để thêm chúng từ working directory sang staging area. Sau đó sử dụng câu lệnh git commit để lưu từ staging area vào local repository.
Sau đó dùng câu lệnh push để lưu trữ lên remote repo.

## _Cú Pháp Cơ Bản_

### 1. Git init

```sh
git init trong thư mục gốc của dự án.
```

Khởi tạo 1 git repository 1 project mới hoặc đã có.

### 2. Git clone

```sh
git clone <:clone git url:>
```

Copy 1 git repository từ remote source.

### 3. Branch

```sh
git branch hoặc git branch -a
```

Các Branch (nhánh) đại diện cho các phiên bản cụ thể của một kho lưu trữ tách ra từ project chính của bạn.
Branch cho phép bạn theo dõi các thay đổi thử nghiệm bạn thực hiện đối với kho lưu trữ và có thể hoàn nguyên về các phiên bản cũ hơn.

### 4. Commit

```sh
git commit -m ”Đây là message, bạn dùng để note những thay đổi để sau này dễ dò lại”
```

Một commit đại diện cho một thời điểm cụ thể trong lịch sử dự án của bạn. Sử dụng lệnh commit kết hợp với lệnh git add để cho git biết những thay đổi bạn muốn lưu vào local repository.

### 5. Checkout

```sh
git checkout <: branch:>
```

Sử dụng lệnh git checkout để chuyển giữa các branch. Chỉ cần nhập git checkout theo sau là tên của branch bạn muốn chuyển đến hoặc nhập git checkout master để trở về branch chính (master branch).

### 6. Fetch

```sh
git fetch origin master
```

Lệnh git fetch tìm nạp các bản sao và tải xuống tất cả các tệp branch vào máy tính của bạn. Sử dụng nó để lưu các thay đổi mới nhất vào kho lưu trữ của bạn. Nó có thể tìm nạp nhiều branch cùng một lúc.

### 7. Fork

Một fork là một bản copy của một repository (Kho chứa source code của bạn trên Github). Việc fork một repository cho phép bạn dễ dàng chỉnh sửa, thay đổi source code mà không ảnh hưởng tới source gốc. 

Một ví dụ về việc sử dụng fork, là khi bạn muốn fix bug source code trên repository của một ai đó, khi đó bạn cần thực hiện theo quy trình sau:

Fork repository đó về tài khoản Github của mình
Thực hiện fix bug
Gửi một Pull Request tới repository gốc
Khi chủ sở hữu của repository nơi bạn fork, sẽ review chỉnh sửa của bạn, và tiến hành merge nội dung chỉnh sửa vào source gốc

![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/git_img_7.png)

### 8. Head

```sh
cat .git/HEAD
```
Khi làm việc với Git, chỉ một branch có thể check out tại một thời điểm - và đây được gọi là nhánh "HEAD". Thông thường, đây còn được gọi là nhánh "active" hoặc "current".

Git ghi chú về branch hiện tại này trong một tệp nằm bên trong kho lưu trữ Git, trong .git / HEAD

### 9. Index

```sh
git status
```

Bất cứ khi nào bạn thêm, xóa hoặc thay đổi một file, nó vẫn nằm trong chỉ mục cho đến khi bạn sẵn sàng commit các thay đổi. Nó như là khu vực tổ chức (stagging area) cho Git. Sử dụng lệnh git status để xem nội dung của index của bạn.

![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/git_img_4.png)

### 10. Master

Master là nhánh chính của tất cả các repository của bạn. Nó nên bao gồm những thay đổi và commit gần đây nhất.

![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/git_img_5.jpg)

### 11. Merge

```sh
git merge <:branch_ban_muon_merge:>
```

Lệnh git merge kết hợp với các yêu cầu kéo (pull requests) để thêm các thay đổi từ nhánh này sang nhánh khác.

### 12. Origin

![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/git_img_6.jpg)

Origin là phiên bản mặc định của repository. Origin cũng đóng vai trò là bí danh hệ thống để liên lạc với nhánh chính.

Lệnh git push origin master để đẩy các thay đổi cục bộ đến nhánh chính.

### 13. Pull

```sh
git pull <:remote:> <:branch:>
```

Pull requests thể hiện các đề xuất thay đổi cho nhánh chính. Nếu bạn làm việc với một nhóm, bạn có thể tạo các pull request để yêu cầu người bảo trì kho lưu trữ xem xét các thay đổi và hợp nhất chúng.

Lệnh git pull được sử dụng để thêm các thay đổi vào nhánh chính.

### 14. Push

```sh
git push <:remote:> <:branch:>
```

Lệnh git push được sử dụng để cập nhật các nhánh từ xa với những thay đổi mới nhất mà bạn đã commit.

### Phân biệt Fetch & Pull
```sh
Lệnh git pull $remote_origin $branch_name sẽ tải về (hay fetch) dữ liệu từ một branch duy nhất $branch_name từ remote server và sau đó merge các thay đổi từ remote này vào repository dưới local.

Ngược lại git fetch $remote_origin sẽ tải về (fetch) dữ liệu của toàn bộ các branch trên URL quy định bởi $remote_origin nhưng không thực hiện việc merge các thay đổi này vào local.
```

### 15. Rebase

Lệnh git rebase cho phép bạn phân tách, di chuyển hoặc thoát khỏi các commit(https://www.codehub.com.vn/Tim-Hieu-Ve-Rebase-Trong-Git)

### 16. Remote

```sh
git remote để kiểm tra và liệt kê. Và git remote add <: remote_url:> để thêm.
```

Lệnh git remote cho phép bạn tạo, xem và xóa kết nối giữa các Repo. Những kết nối chứa thông tin địa chỉ đến kho chứa khác, nó có một cái tên để sử dụng khi cần thiết.

### 17. Repository

Kho lưu trữ Git chứa tất cả các tệp dự án của bạn bao gồm các branch, tags và commit.

### 18. Stash
```sh
git stash save "dang code lo do cho feature #0001"
```
Dùng để lưu trữ tạm thời những  gì bạn đang làm và làm việc khác trong một khoảng thời gian.

### 19. Tag

```sh
git tag v1.0.0
```

Tag là chức năng đặt tên một cách đơn giản của Git, nó cho phép ta xác định một cách rõ ràng các phiên bản mã nguồn (code) của dự án. Ta có thể coi tag như một branch không thay đổi được. Một khi nó được tạo (gắn với 1 commit cụ thể) thì ta không thể thay đổi lịch sử commit ấy được nữa.

### 20. Upstream

Trong ngữ cảnh của Git, upstream đề cập đến nơi bạn push các thay đổi của mình, thường là nhánh chính (master branch).

### 21. Git add

```sh
git add
```

Thêm thay đổi đến stage/index trong thư mục làm việc.

### 22. Git reset

```sh
git reset HEAD tên_file
```

 Bạn đã đưa một tập tin nào đó vào Staging Area nhưng bây giờ bạn muốn loại bỏ nó ra khỏi đây để không phải bị commit theo.
 
## _GitHub_

Dựa vào tên gọi thì các bạn cũng có thể đoán được rằng Git và GitHub có liên quan đến nhau.
ta biết rằng các thành viên của một dự án đồng bộ code với nhau thông qua một remote repository được lưu trữ trên máy chủ riêng biệt
GitHub là một công ty cung cấp dịch vụ lưu trữ các dự án được kiểm soát phiên bản bởi Git, có trang chủ tại địa chỉ https://github.com/.

Ngoài mục đích chính là nơi để lưu trữ các dự án sử dụng Git, GitHub là một nơi có thể giúp các lập trình viên cải thiện kĩ năng của bản thân. Hiện nay, GitHub là một trong những dịch vụ phổ biến nhất để lưu trữ các dự án mã nguồn mở. Các lập trình viên khác có thể tham gia đóng góp vào các dự án này thông qua việc sử dụng Git và GitHub.

## _Demo project thực tế tmm_

![](https://raw.githubusercontent.com/ari-technology/document/master/trung/images/git_img_22.png)

## License
Version 1.0
**Trung.nb!**
