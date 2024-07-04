<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Notification App</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Notification Center</h1>
        <button id="notifyBtn">Show Notification</button>
    </div>
    <script src="script.js"></script>
</body>
</html>
<style>
    body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    margin: 0;
    padding: 0;
}

.container {
    text-align: center;
    margin-top: 20%;
}

button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
}

</style>
<script>
    document.getElementById('notifyBtn').addEventListener('click', () => {
    if (Notification.permission === 'granted') {
        showNotification();
    } else if (Notification.permission !== 'denied') {
        Notification.requestPermission().then(permission => {
            if (permission === 'granted') {
                showNotification();
            }
        });
    }
});

function showNotification() {
    new Notification('Notification App', {
        body: 'This app is running!',
        icon: 'icon.png'
    });
}
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('service-worker.js')
    .then(function(registration) {
        console.log('Service Worker registered with scope:', registration.scope);
    }).catch(function(error) {
        console.log('Service Worker registration failed:', error);
    });
}
document.getElementById('notifyBtn').addEventListener('click', () => {
    fetch('notification-data.json')
        .then(response => response.json())
        .then(data => {
            if (Notification.permission === 'granted') {
                showNotification(data.title, data.body);
            } else if (Notification.permission !== 'denied') {
                Notification.requestPermission().then(permission => {
                    if (permission === 'granted') {
                        showNotification(data.title, data.body);
                    }
                });
            }
        });
});

function showNotification(title, body) {
    new Notification(title, {
        body: body,
        icon: 'icon.png'
    });
}

</script>
