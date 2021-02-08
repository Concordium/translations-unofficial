
.. _Discord: https://discord.gg/xWmQ5tp

.. _testnet-get-started:

=======================================
Concordium ID: Làm quen với ứng dụng
=======================================

.. contents::
   :local:
   :backlinks: none

Trước khi làm theo hướng dẫn này, hãy chắc chắn rằng bạn đã cài đặt thành công Concordium ID theo :ref:`the previous chapter<testnet-get-the-app>`.

Thiết lập mật mã và sinh trắc học
================================

Khi mở ứng dụng Concordium ID lần đầu tiên, bạn sẽ được yêu cầu thiết lập mật mã và xác thực sinh trắc học, tạo một :ref:`glossary-initial-account`, nó cũng sẽ hướng dẫn bạn nhận được :ref:`glossary-identity`. Initial Account là loại tài khoản đặc biệt, được gửi đến chuỗi bởi :ref:`glossary-identity-provider` khi tạo mới. Bạn có thể gửi giao dịch từ tài khoản này hoặc tài khoản thông thường, nhưng Identity Provider chỉ xác thực thông tin của Initial Account. Sau khi tạo Identity, bạn có thể tự tạo các tài khoản mới, những tài khoản này sẽ không được xác thực bởi Identity Provider. Bạn có thể tìm hiểu thêm về :ref:`Identities and accounts<reference-id-accounts>`

Màn hình đầu tiên khi mở ứng dụng Concordium ID cho biết một số thông tin cần thiết lập trước khi bắt đầu.

Nếu bạn đã sẵn sàng, nhấn vào **Yes, let’s go!**. Tiếp theo, hãy tạo mật khẩu gồm 6 chữ số. Nếu muốn sử dụng mật khẩu có chữ cái, bạn có thể chọn **Use full password instead**

.. image:: images/concordium-id/int1.png
      :width: 32%
.. image:: images/concordium-id/int2.png
      :width: 32%

Tạo mật khẩu xong, bạn có thể sử dụng sinh trắc học nếu điện thoại hỗ trợ, ví dụ: nhận diện khuân mặt, vân tay. Chúng tôi khuyên bạn nên sử dụng nếu điện thoại hỗ trợ.

.. image:: images/concordium-id/int3.png
      :width: 32%
      :align: center

Tạo initial account và identity
=========================================

Tiếp theo, bạn sẽ được lựa chọn tạo mới initial account và identity hoặc khôi phục tài khoản cũ.
Giả sử đây là lần đầu tiên bạn sử dụng Concordium ID, chọn **I want to create my initial account** để tiếp tục.

.. image:: images/concordium-id/int4.png
      :width: 32%
      :align: center

Tại màn hình tiếp theo, bạn sẽ thấy mô tả và ba bước tạo initial account, cùng với identity. Hiểu đơn giản, initial account là tài khoản được gửi đến chain thông qua identity provider, do vậy họ sẽ biết bạn là chủ sở hữu của tài khoản đó. Sau đó, bạn có thể tự tạo mới tài khoản, nghĩa là chỉ bạn mới biết thông tin của những tài khoản này.

.. image:: images/concordium-id/int5.png
      :width: 32%
      :align: center
	  
Ba bước là:

1. Đặt tên cho initial account
2. Đặt tên cho identity
3. Yêu cầu initial account và identity từ :ref:`glossary-identity-provider` bạn đã chọn

Bạn sẽ bắt đầu bước đầu tiên ở trang tiếp theo, nhập tên cho tài khoản initial account của bạn. Ấn **Continue** sẽ đưa bạn sang bước tiếp theo, nhập tên cho identity. Cả hai tên này đều chỉ mình bạn biết, nên bạn thích gì đặt đấy (Có một số yêu cầu về chữ cái và ký tự có thể sử dụng)

Trong ví dụ này, chúng ta sẽ đặt tên cho initial account là *Example Account 1* và identity là *Example Identity*. Như đã nói ở trên, bạn có thể đặt tên là bất cứ thứ gì bạn muốn.
 
.. image:: images/concordium-id/int6.png
      :width: 32%
.. image:: images/concordium-id/int7.png
      :width: 32%

Nhấn **Continue to identity providers**, bạn sẽ được đưa sang bước tiếp theo để lựa chọn *identity providers*. Identity Providers là một thực thể ở bên ngoài, họ sẽ xác minh bạn là ai, trước khi trả về một đối tượng nhận dạng được sử dụng trên chuỗi.
Hiện tại, bạn có thể lựa chọn giữa:

* *Notabene Development*: cung cấp cho bạn danh tính thử nghiệm mà không cần xác minh ngoài đời thực
* *Notabene*: xác minh danh tính thật của bạn

.. image:: images/concordium-id/int8.png
      :width: 32%
      :align: center

Lựa chọn Notebene Development, bạn sẽ nhận được danh tính thử nghiệm mà không cần làm gì. Nếu chọn Notabene, bạn nhận được quy trình xác minh danh tính của họ, quy trình này sẽ hướng dẫn bạn cách xác minh danh tính thật.
Sau khi xong, bạn sẽ được đưa trở lại Concordium ID.

Khi hoàn thành một trong hai quy trình cấp danh tính, bạn thấy màn hình sau. Đây là tổng quan về tài khoản initial account và identity của bạn.

.. image:: images/concordium-id/int9.png
      :width: 32%
      :align: center

Tùy vào identity provider bạn chọn, thẻ danh tính có thể khác đôi chút. Bạn có thể thấy Example Account 1 nằm trong Example Identity. Tài khoản được tạo ra tại bước này sẽ được đánh dấu *(Initial)* trong ứng dụng, bạn có thể nhận biết tài khoản nào là initial account.

Nhấn **Finish** sẽ chuyển bạn đến *Accounts*. Tại đây, bạn có thể thấy tài khoản initial account vừa được tạo. Có thể xuất hiện *Pending icon*, nghĩa là Identity Provider đang làm việc để gửi, tạo initial account và identity. Bạn có thể chuyển sang *Identities* bằng cách nhấn vào **Identities** ở góc dưới màn hình. Tại đây, bạn sẽ thấy identity vừa tạo của bạn, có thể vẫn đang chờ xử lý nếu identity provider chưa làm xong. Điều bạn cần làm bây giờ là chờ đến khi chúng hoàn thành.


.. image:: images/concordium-id/int10.png
      :width: 32%
.. image:: images/concordium-id/int11.png
      :width: 32%


Hỗ trợ & phản hồi
==================

Nếu bạn gặp vấn đề nào hoặc có đề xuất, hãy đăng lên `Discord`_ hoặc gửi mail đến testnet@concordium.com
