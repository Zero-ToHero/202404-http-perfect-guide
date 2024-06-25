- **엔진엑스 (Nginx)**:
    - **Rewrite Module**: `rewrite` 지시자를 사용하여 다양한 리다이렉션을 설정
        
        ```
        
        server {
            listen 80;
            server_name example.com;
            rewrite ^/oldpage$ http://new.example.com/newpage permanent;
        }
        
        ```
        
    
    **Return Directive**: `return` 지시자를 사용하여 리다이렉션을 수행
    
    ```
    nginx코드 복사
    server {
        listen 80;
        server_name example.com;
        location /oldpage {
            return 301 http://new.example.com/newpage;
        }
    }
    
    ```
    
- **클라우드플레어 (Cloudflare)**:
    - **URL 리다이렉션**
    - **DNS Only (DNS 전용 설정)**:
        - 클라우드플레어에 도메인을 추가하고 DNS Only 모드로 설정하면 클라우드플레어 서버를 경유하지 않고 순수하게 DNS 서비스만 제공
    - **프록시 설정 (Proxy)**:
        - 클라우드플레어에 도메인을 추가하고 프록시(Proxy) 모드로 설정하면 클라우드플레어의 CDN 기능을 활용하여 웹 트래픽을 보호하고 최적화할 수 있
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/9bf36ad3-0719-431a-b1e2-3f429aeb690d/4686f5cb-b3b1-40dc-9223-8a7fd0a63606/Untitled.png)
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/9bf36ad3-0719-431a-b1e2-3f429aeb690d/b5572260-7296-4e49-a098-9f086f429392/Untitled.png)
    
- AWS
    - **Route 53**
        - NS 서비스인 Route 53을 사용하여 특정 도메인 또는 서브도메인을 다른 URL로 리다이렉션
    - **ALB, NLB, CLB**
        - 로드 밸런서를 사용하여 특정 경로나 도메인을 다른 URL로 리다이렉션
- HTML
    - **<meta http-equiv="refresh" content="0;url=https://example.com/newpage">**
        
        `<meta>` 태그를 사용하여 리다이렉션을 수행
        
- JavaScript
    - **window.location.href = "https://example.com/newpage";**
    
- Spring Boot
    
    ```jsx
        @GetMapping("/oldpage")
        public String redirectToNewPage() {
            // 다음과 같이 "redirect:" 접두어를 사용하여 리다이렉션할 URL을 지정합니다.
            return "redirect:/newpage";
        }
    ```