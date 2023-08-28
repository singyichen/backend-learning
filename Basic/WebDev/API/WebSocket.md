---
title: WebSocket
description: 
published: true
date: 2023-08-25T00:09:20.680Z
tags: websocket
editor: markdown
dateCreated: 2023-05-18T09:13:03.754Z
---

# WebSocket
- [ ] [Short/long polling, SSE, WebSocket](https://blog.bytebytego.com/p/ep-39-accounting-101-in-payment-systems?utm_source=profile&utm_medium=reader2)
- [ ] [Understanding Websockets In depth](https://vishalrana9915.medium.com/understanding-websockets-in-depth-6eb07ab298b3)
- [ ] [Making a multiplayer web game with websocket that can be scalable to millions of users](https://medium.com/@dragonblade9x/making-a-multiplayer-web-game-with-websocket-that-can-be-scalable-to-millions-of-users-923cc8bd4d3b)
- [ ] [Network Protocols behind Server Push, Online Gaming, and Emails](https://blog.bytebytego.com/p/network-protocols-behind-server-push?utm_source=profile&utm_medium=reader2)
- [ ] [網頁即時通訊實作 X Long-Polling, Server Sent Events, WebSockets](https://linyencheng.github.io/2022/09/23/relationships-between-frontend-and-backend/js-realtime-communication/)
- [ ] [WebSocket 基本介紹及使用筆記](https://www.letswrite.tw/websocket/)
- [ ] [淺談 WebSocket 協定：實作一個簡單的即時聊天室吧！](https://hackmd.io/@Heidi-Liu/javascript-websocket)
- [ ] [JavaScript | WebSocket 讓前後端沒有距離](https://medium.com/enjoy-life-enjoy-coding/javascript-websocket-%E8%AE%93%E5%89%8D%E5%BE%8C%E7%AB%AF%E6%B2%92%E6%9C%89%E8%B7%9D%E9%9B%A2-34536c333e1b)
- [ ] [【Web微知識系列】雙向溝通的技術，什麼是Websocket？](https://vocus.cc/article/627280c5fd89780001c71e35)
- [ ] [React | 在 React 中使用 WebSocket - feat. Socket.io 基本教學](https://ms314006.github.io/use-websocket-by-react-socket-io/)
- [ ] [Connect SocketIO Server with Postman Client](https://www.youtube.com/watch?v=RWrNL-I3j7k&ab_channel=SWIKbyMirTahaAli&loop=0)
- [ ] [[course] 即使通訊與傳輸（realtime、streaming、websocket）](https://pjchender.dev/webdev/course-fem-realtime/)
- [ ] [[note] socket.io 筆記](https://pjchender.dev/npm/npm-socket-io/#%E6%9C%AA%E9%96%B1%E8%AE%80)
- [ ] [用 Socket.io 做一個即時聊天室吧！（直播筆記）](https://creativecoding.in/2020/03/25/%E7%94%A8-socket-io-%E5%81%9A%E4%B8%80%E5%80%8B%E5%8D%B3%E6%99%82%E8%81%8A%E5%A4%A9%E5%AE%A4%E5%90%A7%EF%BC%81%EF%BC%88%E7%9B%B4%E6%92%AD%E7%AD%86%E8%A8%98%EF%BC%89/)
- [ ] [使用 Node.js 與 Socket.IO 建立即時性（Realtime）網頁應用程式 App](https://blog.gtwang.org/programming/socket-io-node-js-realtime-app/)
- [ ] [Socket.io 的架構](https://mark-lin.com/posts/20170913/)
- [ ] [Socket.io 的說話島](https://mark-lin.com/posts/20170914/)

![WebSocket 重點心智圖.png](http://192.168.25.60:8000/files/file_storage/e9c7606b.png)

![Short long polling SSE WebSocket.png](http://192.168.25.60:8000/files/file_storage/61384721.png)

![How to Establish a WebSocket Connection.png](http://192.168.25.60:8000/files/file_storage/4913e244.png)

![WebSocket Message.png](http://192.168.25.60:8000/files/file_storage/1105ad71.png)

# Socket.IO
> - Socket.IO 是一個用於建立即時性通訊網頁應用程式（realtime web applications）的跨平台 JavaScript 函式庫，可以消除不同平台上傳輸方式的差異性，讓開發者更容易發展即時性的網頁應用程式。
> - Socket.IO 包含瀏覽器端函式庫（client-side library，運行於瀏覽器中）與伺服器端函式庫（server-side library，運行於 Node.js 環境），而兩者所提供的 API 幾乎相同。
> - 在傳輸的方式上，Socket.IO 使用 WebSocket 作為主要的傳輸協定，而在某些瀏覽器不支援 WebSocket 的狀況下，則會自動改用其他的方式來傳輸（如 Adobe Flash sockets、JSONP polling 與 AJAX long polling 等），至於 API 的使用方式則維持不變，也就是說開發者可以不必考慮該使用哪一種傳輸方式，Socket.IO 會自動選擇一個最適合的來使用。
> - 在大部分新的瀏覽器中，Socket.IO 其實都是使用 WebSocket 來傳輸，所以 Socket.IO 也可以視為一個 WebSocket 的包裝工具，但是他所提供的功能比 WebSocket 還要豐富，例如 Socket.IO 的 heartbeats、timeouts 與 disconnection 等功能對於即時性的應用程式而言都是很重要的，但是原生的 WebSocket API 卻沒有這些功能。





