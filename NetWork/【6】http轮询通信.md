# 【6】http轮询通信
- 隔一段时间发一次（setInterval）不能保证发送顺序和接受顺序。
- 接收到上次查询结果后，隔一定时间发一次（setTimeout）可以保证接受顺序。
- 长轮询，建立连接后始终保持连接（代价较高）。
<u>由于http每次都要建立连接和保持连接代价较高，websocket避免了http的这种缺点。</u>