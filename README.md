# howtodeployprojectsinsite
how to deploy projects in the sites by making the services

Note- open the cmd in the administrator and run this E:\nssm-2.24\nssm-2.24\win64\nssm.exe install it will open the gui. just select the exe whose services you want to make

# densofinalcompleteproject
we have made the complete denso project with rfid

Note- isme esd ki wajah se sare rs485 heat ho rhe the aur data transfer hona ruk jaa rha tha to iska solution ye nikla ki jo mai esd ka signal utha rha tha pin number 7 se arduino par directly. to mai ab directly naa utha kar waha relay connect kar diya aur ab us relay se signal utha rha hu aur yahi same chiz mai diode laga kar bhi kar sakta hu


step-1) run the apache and mysql in the background as a services
net start MySQL         :: Start MySQL service
net stop MySQL          :: Stop MySQL service
sc delete MySQL         :: Delete the MySQL service (be careful)

step-2) run apache as a services
cd C:\xampp\apache\bin httpd.exe -k install -n "ApacheXAMPP"
net start ApacheXAMPP

or other method to do it
1) for mysql
cd C:\xampp\mysql\bin mysqld.exe --install MySQL
net start MySQL

3) for apache
cd C:\xampp\apache\bin httpd.exe -k install -n "Apache2.4"
net start Apache2.4

Note- make the exe of you backned
1) make one file named run_server.py and put this code
# run_server.py
from waitress import serve
from denso.wsgi import application
import logging
print("🔧 Starting Waitress server on http://0.0.0.0:8000")
logging.basicConfig(level=logging.INFO)
serve(application, host='0.0.0.0', port=8000)

# run_server.py another one if the first one is not working
import logging
import traceback
from waitress import serve
from anpr_backened.wsgi import application

# Log to file
logging.basicConfig(
    filename="server.log",
    filemode="w",
    level=logging.DEBUG,
    format="%(asctime)s [%(levelname)s] %(message)s"
)

print("🔧 Starting Waitress server on http://0.0.0.0:8000")
logging.info("Starting Waitress server...")

try:
    serve(application, host='0.0.0.0', port=8000)
except Exception as e:
    logging.error("❌ Exception occurred while running server:")
    logging.error(traceback.format_exc())
    print("❌ Server crashed! Check server.log")


1) venv\Scripts\activate
2) make the exe
pyinstaller --onefile --noconsole ^
--hidden-import=authentication ^
--hidden-import=authentication.apps ^
--hidden-import=authentication.urls ^
--hidden-import=rfid_esd ^
--hidden-import=rfid_esd.apps ^
--hidden-import=rfid_esd.urls ^
--hidden-import=dj_rest_auth ^
--hidden-import=dj_rest_auth.registration ^
--hidden-import=allauth ^
--hidden-import=allauth.account ^
--hidden-import=allauth.socialaccount ^
--hidden-import=rest_framework ^
run_server.py

how to setup django backened code in services so that it runs in the background

step-1 we have the backened exe
step-2 after making the backened exe i want to make the services of this exe so to make it. open the cmd in the administrator
a) nssm install MyPythonService or E:\nssm-2.24\nssm-2.24\win64\nssm.exe install (yeh hum tb chalayenge jab humara nssm.exe kisi folder ke andar hoga)
b) so one gui is open then i will select the my exe and write the services name densobackenedservices now i need to make the start the services
c) nssm start densobackenedservices
d) for stop the services sc stop densobackenedservices
e) for deleting the services sc delete densobackenedservices

so backened services is done and also working

2)
a) not make the services for the frotened when made by the vite.
i have made paramount controller for controlling the barrier
frontened ka exe kaise banate hai. so hum windows ke exe ke liye electron ka use karte hai. lekin agar hum chahte hai ki mera ek exe ho jisse mera frontened windows me open na hokar browser me open ho to mujhe ye chiz apply karni padegi.

step-0) npm install -g pkg step-1) npm install serve-handler step-2) sabse pahle ek server.js ke name se file banao. jo ki main project ke andar ye file rahegi.

const { exec } = require('child_process'); exec('npx serve -s dist -l 3000'); exec('start http://localhost:3000');

step-3) run the npm run build.

step-4) pkg server.js --targets node18-win-x64 --output SaralaxeIndia.exe

step-5) agar hume client ka image se exe banani hai to hum wo bhi kar sakte hai

step-6) isko run krne ke liye dist folder chahiye aur dist folder ke bahar ye meri exe hono chahiye tbhi ye exe jo hai dist folder me jo files hai unko serve karti hai browser me means ki meri website ko, aur mera package.json file bhi hona chahiye dependency ke liye. so agar mujhe kisi client ke pc me ye chiz dalni hai to mere pass dist folder, exe aur package.json file hona chahiye

b)
not make the services for the frotened when made by the create react app.

frontened ka exe kaise banate hai. so hum windows ke exe ke liye electron ka use karte hai. lekin agar hum chahte hai ki mera ek exe ho jisse mera frontened windows me open na hokar browser me open ho to mujhe ye chiz apply karni padegi.

step-0) npm install -g pkg step-1) npm install serve-handler step-2) sabse pahle ek server.js ke name se file banao. jo ki main project ke andar ye file rahegi.

step-3) server.js
const { spawn } = require("child_process");
const open = require("open");

const server = spawn("npx", ["serve", "-s", "build", "-l", "3000"], {
  shell: true,
  stdio: "pipe",
});

server.stdout.on("data", (data) => {
  const output = data.toString();
  console.log([stdout] ${output});

  if (output.includes("Accepting connections at")) {
    open("http://localhost:3000");
  }
});

server.stderr.on("data", (data) => {
  console.error([stderr] ${data});
});

server.on("close", (code) => {
  console.log(Child process exited with code ${code});
});

step-3) run the npm run build.

step-4) pkg server.js --targets node18-win-x64 --output SaralaxeIndia.exe

step-5) agar hume client ka image se exe banani hai to hum wo bhi kar sakte hai

step-6) isko run krne ke liye dist folder chahiye aur dist folder ke bahar ye meri exe hono chahiye tbhi ye exe jo hai dist folder me jo files hai unko serve karti hai browser me means ki meri website ko, aur mera package.json file bhi hona chahiye dependency ke liye. so agar mujhe kisi client ke pc me ye chiz dalni hai to mere pass dist folder, exe aur package.json file hona chahiye

3rd way to make the exe without using the dist folder. above me frontend ke humne jo bhi ways dekhe hai usme hume dist folder ko rakhna padta hai. lekin agar mai chahta hu ki mujhe dist folder ke bina hi mera exe chale to mai simply server.js me ye wala code run karunga. aur baki ke sabhi approach same hi rhenge
server.js
// server.js
const http = require("http");
const path = require("path");
const fs = require("fs");
const { exec } = require("child_process");

// Helper: Get content type
function getContentType(filePath) {
  const ext = path.extname(filePath);
  if (ext === ".js") return "application/javascript";
  if (ext === ".css") return "text/css";
  if (ext === ".html") return "text/html";
  if (ext === ".json") return "application/json";
  if (ext === ".ico") return "image/x-icon";
  if (ext === ".png") return "image/png";
  return "text/plain";
}

// Serve files from virtual memory (pkg embeds dist folder)
const baseDir = path.join(__dirname, "dist");

const server = http.createServer((req, res) => {
  const reqPath = req.url === "/" ? "/index.html" : req.url;
  const filePath = path.join(baseDir, reqPath);

  fs.readFile(filePath, (err, data) => {
    if (err) {
      // SPA fallback
      fs.readFile(path.join(baseDir, "index.html"), (err, fallback) => {
        if (err) {
          res.writeHead(404);
          res.end("404 Not Found");
        } else {
          res.writeHead(200, { "Content-Type": "text/html" });
          res.end(fallback);
        }
      });
    } else {
      res.writeHead(200, { "Content-Type": getContentType(filePath) });
      res.end(data);
    }
  });
});

server.listen(3000, () => {
  console.log("Server running at http://localhost:3000");
  exec("start http://localhost:3000");
});

Note- c sharp me jo exe hai usse agar hume services banana hai to hume simply cmd ko administrator me chalana hai aur ye likhna hai
a) for creating the services-     sc create MyTestService binPath= "C:\Services\MyService.exe"
b) for stopping the services-     sc stop MyTestService
c) for deleting the services-     sc delete MyTestService
