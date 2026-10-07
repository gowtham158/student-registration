# Student Registration Web Application on AWS EC2

A simple web application built using **HTML, CSS, JavaScript, Nginx, Python Flask, and AWS EC2**. The frontend is hosted using Nginx, while the Flask backend provides REST APIs to register students and retrieve student information.

## 🚀 Project Overview

This project demonstrates how to deploy and connect a frontend and backend on an AWS EC2 Ubuntu server using Nginx as a web server and reverse proxy.

Users can enter a student's name and email address through a web form. The frontend sends requests to the Flask backend, which processes the data and returns JSON responses.

## 🏗️ Architecture

```text
             User Browser
                  |
                  v
          HTML / CSS / JavaScript
                  |
                  v
             Nginx Server
              Port 80
                  |
           /api/ requests
                  |
                  v
           Python Flask API
            127.0.0.1:5000
                  |
                  v
       In-Memory Student Records

       All services run on AWS EC2
```

## 🛠️ Technologies Used

- **HTML5** – Web page structure
- **CSS3** – Frontend styling
- **JavaScript** – API requests and dynamic updates
- **Nginx** – Web server and reverse proxy
- **Python** – Backend programming
- **Flask** – REST API development
- **AWS EC2** – Cloud hosting
- **Ubuntu Linux** – Server operating system
- **Git and GitHub** – Version control and project hosting

## ✨ Features

- Student registration form
- Name and email input validation
- Display registered students
- REST API integration between frontend and backend
- Nginx reverse proxy configuration
- Backend accessible through localhost
- Cloud deployment on AWS EC2

## 📁 Project Structure

```text
student-registration-app/
│
├── frontend/
│   ├── index.html
│   └── style.css
│
├── backend/
│   └── app.py
│
├── nginx/
│   └── student-app.conf
│
└── README.md
```

*Note: Adjust the folder structure above to match your actual GitHub repository. In the deployment setup, the frontend files are served from `/var/www/student-app`, and the Nginx configuration is stored under `/etc/nginx/sites-available/`.*

## ⚙️ Installation and Deployment

### 1. Launch an AWS EC2 instance

- Launch an Ubuntu EC2 instance.
- Configure SSH access using a key pair.
- Allow inbound HTTP traffic on port `80`.
- Restrict SSH port `22` to your IP address.

### 2. Install the required packages

```bash
sudo apt update
sudo apt install nginx python3 python3-pip python3-venv -y
```

Start Nginx:

```bash
sudo systemctl enable --now nginx
```

### 3. Configure the frontend

Create the frontend directory:

```bash
sudo mkdir -p /var/www/student-app
sudo chown -R $USER:$USER /var/www/student-app
```

Place `index.html` and `style.css` inside this directory.

### 4. Set up the Flask backend

```bash
mkdir -p ~/student-backend
cd ~/student-backend

python3 -m venv venv
source venv/bin/activate

pip install flask
```

Place the backend code in `app.py` and start the application:

```bash
python app.py
```

The Flask application listens on `127.0.0.1:5000`.

### 5. Configure Nginx

Create the configuration file:

```bash
sudo nano /etc/nginx/sites-available/student-app
```

Example configuration:

```nginx
server {
    listen 80;
    server_name _;

    root /var/www/student-app;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:5000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Enable the site:

```bash
sudo ln -s /etc/nginx/sites-available/student-app /etc/nginx/sites-enabled/student-app
sudo rm /etc/nginx/sites-enabled/default

sudo nginx -t
sudo systemctl reload nginx
```

### 6. Access the application

Open your browser and visit:

```text
http://YOUR_EC2_PUBLIC_IP
```

Replace `YOUR_EC2_PUBLIC_IP` with your EC2 instance's public IPv4 address.

## 🔌 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/api/students` | Retrieve student records |
| POST | `/api/students` | Register a student |

Example request body:

```json
{
  "name": "Arun Kumar",
  "email": "arun@example.com"
}
```

Test the API through Nginx:

```bash
curl http://127.0.0.1/api/students
```

## 🔐 Security Considerations

- Restrict SSH access to trusted IP addresses.
- Keep the Flask backend bound to localhost.
- Do not expose port `5000` publicly.
- Use HTTPS for production deployments.
- Avoid storing sensitive information in the demonstration application.

## ⚠️ Limitations

- Student records are stored in memory and disappear when the backend restarts.
- No database is configured.
- Authentication and authorization are not implemented.
- The example is intended for learning and demonstration purposes.

## 🔮 Future Enhancements

- Integrate MySQL or Amazon RDS for persistent storage.
- Add student update and delete functionality.
- Configure HTTPS with a TLS certificate.
- Run Flask using Gunicorn and manage it with systemd.
- Add monitoring and centralized logging.

## 🎯 Learning Outcomes

- Deploy a web application on AWS EC2.
- Host static files using Nginx.
- Configure Nginx as a reverse proxy.
- Build and test REST APIs using Flask.
- Connect a frontend to a backend.
- Configure Linux services and AWS Security Groups.
- Troubleshoot basic cloud deployment issues.

## 👨‍💻 Author

**Bhuvanesh M**

- GitHub: [Bhuvan-here](https://github.com/Bhuvan-here)
- LinkedIn: [Bhuvanesh M](https://www.linkedin.com/in/bhuvanesh-m-347310250/)

---

⭐ If you find this project useful, feel free to star the repository.
