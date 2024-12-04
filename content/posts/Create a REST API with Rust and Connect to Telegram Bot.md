---
title: Create a REST API with Rust and Connect to Telegram Bot
date: 2024-12-04
draft: false
tags:
  - rust
  - mitoyus
  - blog
  - telegram
  - chatgpt
---
To create a REST API with Rust that allows you to send photos and videos to a Telegram bot, we can use the Actix Web framework for the API and the Teloxide library for interacting with the Telegram bot.

### Steps:
1. **Set up Actix Web** to create the REST API.
2. **Use Teloxide** to interact with the Telegram API for sending media.
3. **Use multipart form data** to handle file uploads in the REST API.

### Dependencies:
You will need the following dependencies in `Cargo.toml`:

```toml
[dependencies]
actix-web = "4.0.0-beta.5"
actix-rt = "2.5.0"
teloxide = "0.7"
tokio = { version = "1", features = ["full"] }
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
dotenv = "0.15"
reqwest = { version = "0.11", features = ["multipart"] }
futures-util = "0.3"
```

- **Actix Web** for building the API.
- **Teloxide** for interacting with Telegram.
- **Reqwest** for handling multipart form data.
- **Dotenv** for managing environment variables like the Telegram bot token.

### Step-by-step guide:

#### 1. Create the `main.rs` file

```rust
use actix_web::{web, App, HttpServer, Responder, HttpResponse};
use teloxide::{Bot, prelude::*};
use std::sync::Arc;
use tokio::sync::Mutex;
use futures_util::stream::StreamExt;
use actix_multipart::Multipart;
use actix_web::Error;
use reqwest::Client;

#[derive(Clone)]
struct AppState {
    bot: Bot,
    chat_id: i64, // Chat ID where media will be sent
}

async fn send_media_to_telegram(
    app_state: web::Data<AppState>,
    mut payload: Multipart,
) -> Result<HttpResponse, Error> {
    let client = Client::new();
    
    while let Some(field) = payload.next().await {
        let mut field = field?;

        let filename = field.content_disposition().get_filename().unwrap_or("file");
        let media = reqwest::multipart::Form::new().part(
            "media",
            reqwest::multipart::Part::reader(field)
                .file_name(filename.to_string())
                .mime_str("application/octet-stream")?,
        );

        // Here we send the media to Telegram using the bot instance
        let response = app_state
            .bot
            .send_photo(app_state.chat_id.clone(), media)
            .await;
        
        match response {
            Ok(_) => return Ok(HttpResponse::Ok().body("Media sent successfully")),
            Err(_) => return Ok(HttpResponse::InternalServerError().body("Failed to send media")),
        }
    }

    Ok(HttpResponse::BadRequest().body("No file received"))
}

#[actix_web::main]
async fn main() -> std::io::Result<()> {
    dotenv::dotenv().ok();

    // Load your bot token and chat ID from the environment variables
    let bot_token = std::env::var("TELEGRAM_BOT_TOKEN")
        .expect("TELEGRAM_BOT_TOKEN environment variable must be set");

    let chat_id = std::env::var("TELEGRAM_CHAT_ID")
        .expect("TELEGRAM_CHAT_ID environment variable must be set")
        .parse::<i64>()
        .expect("Invalid chat_id");

    let bot = Bot::new(bot_token);

    let app_state = web::Data::new(AppState {
        bot,
        chat_id,
    });

    HttpServer::new(move || {
        App::new()
            .app_data(app_state.clone())
            .route("/send_media", web::post().to(send_media_to_telegram))
    })
    .bind("0.0.0.0:8080")?
    .run()
    .await
}
```

### Explanation:
1. **`AppState` struct**: This holds the `Bot` instance and the `chat_id` where the media will be sent.
2. **`send_media_to_telegram` function**: This is the handler for the `/send_media` API endpoint. It accepts a multipart request, which is how file uploads are handled. It uses `reqwest::multipart::Form` to prepare the file for upload, and then sends it to the Telegram bot using Teloxide’s `send_photo` function.
3. **Actix Web API Setup**: The API listens on port `8080` and expects a POST request to the `/send_media` endpoint with media data.

#### 2. Environment Setup:
Create a `.env` file to store your bot token and the chat ID (where the bot will send media):

```env
TELEGRAM_BOT_TOKEN=your-telegram-bot-token
TELEGRAM_CHAT_ID=your-chat-id
```

#### 3. Running the Server:

To run the server, use the following command:

```bash
cargo run
```

The server should now be running, and you can send a `POST` request to `http://localhost:8080/send_media` with media (either a photo or video).

#### 4. Sending Media (Example cURL Request):

You can send media using `curl`. Here's an example of how to send a photo:

```bash
curl -X POST http://localhost:8080/send_media -F "media=@/path/to/photo.jpg"
```

To send a video:

```bash
curl -X POST http://localhost:8080/send_media -F "media=@/path/to/video.mp4"
```

#### 5. Telegram Bot Token & Chat ID:

- To get your **Telegram bot token**, create a bot by messaging the BotFather in Telegram.
- To get the **chat ID**, you can message the bot in Telegram and use a simple HTTP request to fetch the chat ID. Here's an example URL to check updates:
  ```
  https://api.telegram.org/bot<your-bot-token>/getUpdates
  ```

This will return the chat ID associated with your bot's messages.

### Conclusion:
- **Actix Web** provides the web server to handle file uploads.
- **Teloxide** sends the uploaded files (photos/videos) to Telegram.
- You can extend this by adding more features, such as handling different types of media or better error handling.