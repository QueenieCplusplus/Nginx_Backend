# Nginx_Backend
後端伺服器


       upstream qs_backend{
       
          ip_hash;
          
          server katesbackend.com;
          server pattysbackend.com;
       
       }


* upstream 指令

   將被代理伺服器集中成為一群組，方便管理。

* ip_hash 指令（實現連現階段的保持）

   跟輪詢搭配加權相對應的方式，當有此指令時，表示代理伺服器會依據（記憶）該使用者之 IP 位址，
   從而進行工作分配之任務，保障了使用者與特定伺服器的連線處於穩定狀態，倘若此指令為 down 關閉狀態，
   則使用者請求可能會被（輪詢後）下一個後端伺服器服務。

------------------------

* 域名跳躍

* 域名鏡像

------------------------

* 位址重定義（產生多次網路請求）

  不同的位址中，選擇正確的位址。
  例如使用者對網址 google.cn 發出請求，伺服器判斷為 www.google.com （完整路徑名稱）的過程。
  當然，此過程後段會產生位址重新導向的階段發生。

* 位址轉發（僅發出一次網路請求）

  作用發生於同一網站專案內部，
  將域名指向到一個已經存在的網站的過程。
  
------------------------

* keepalive [connections] 指令

   該指令可控制網路聯結數量，保障代理伺服器的工作處理程序為後端伺服器群組開放的使用者端網路連接，為其數量做一定控管，
   與 worker_connections 有反相關，後者為單一工作處理程序與用戶端(使用者端)連接的最大上限數。
   
       Keepalive connections can have a major impact on performance by reducing the CPU and network overhead needed to close connections.
       
       NGINX terminates all client（user-side） connections and creates separate and independent connections to the upstream servers. 

       NGINX supports keepalives for both clients and upstream servers. 

      * keepalive_requests – 
      
      The number of requests a client can make over a single keepalive connection. 
      The default is 100, but a much higher value can be especially useful for testing with a load‑generation tool, which generally sends a large number of requests from a single client.

       * keepalive_timeout – 
       How long an idle keepalive connection remains open.

      * keepalive – (relates to upstream keepalives)
      
      The number of idle keepalive connections to an upstream server that remain open for each worker process. 



                            Moduel  |  Data |  File
                            ________|_______|_________
                            Registry|       |  Stack
                            
                            
                                       ||                   Master Process
                                                         /       |         \
                                                           （實現平行處理）
                                                     /           |            \
                         worker threads | slave process   slave process   slave process      全域：設定允許產生的工作處理程序的數量
                         
                                             /||\               /||\             /||\         
                                            / || \             / || \           / || \       事件：connections 單一工作程序能處理的最大連接數設定  65535  
                                           /  ||  \           /  ||   \        /  ||  \    
                                           
                                        ｜｜｜｜｜｜｜ ｜｜｜｜｜｜  ｜｜｜｜｜｜ ｜｜｜｜｜｜ ｜｜   Http: 單連接請求上限數為 100                             





* least_conn 指令

  依據權重，選擇最少（網路）連接（數量）的伺服器。

------------------------

* 複寫規則（與覆寫不同）


