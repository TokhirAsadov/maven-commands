# `Contact`
```java
// Kontaktni tekshirish
        if (update.message() != null && update.message().contact() != null) {
            Contact contact = update.message().contact();
            Long chatID = update.message().from().id(); // Foydalanuvchi ID’sini chat ID sifatida olish

            // Kontakt ulashilgan va foydalanuvchi holati SHARING_PHONE bo‘lsa
            if (userStates.get(chatID) == UserState.SHARING_PHONE) {
                userStates.put(chatID, UserState.ENTERING_EMAIL);
                String welcomeMessage = "Please enter your gmail.\n Example: 123@gmail.com";
                // Keyboardni olib tashlash
                SendMessage sendMessage = new SendMessage(chatID, welcomeMessage)
                        .replyMarkup(new ReplyKeyboardRemove(true));
                bot.execute(sendMessage);
                return; // Keyingi shartlarni tekshirishni to‘xtatish
            }
        }
```
***
# `Photo`
```java
// Check for photo
        if (update.message() != null && update.message().photo() != null && userStates.get(update.message().from().id()) == UserState.SHARING_PHOTO) {
            Long chatID = update.message().from().id();
            PhotoSize[] photos = update.message().photo();
            // Get the largest photo (last in the array)
            PhotoSize largestPhoto = photos[photos.length - 1];
            String fileId = largestPhoto.fileId();

            // Confirm photo received
            String confirmationMessage = "Photo received! Thank you for your submission.";
            bot.execute(new SendMessage(chatID, confirmationMessage));

            // Optional: Download the photo
            try {
                GetFile getFile = new GetFile(fileId);
                GetFileResponse fileResponse = bot.execute(getFile);
                String filePath = fileResponse.file().filePath();
                String downloadUrl = "https://api.telegram.org/file/bot" + bot.getToken() + "/" + filePath;
                System.out.println("Photo download URL: " + downloadUrl);
                // You can use an HTTP client (e.g., OkHttp) to download the file from downloadUrl
            } catch (Exception e) {
                bot.execute(new SendMessage(chatID, "Error processing the photo."));
                e.printStackTrace();
            }

            // Update state (e.g., complete registration)
            userStates.put(chatID, null); // Or move to another state
            return;
        }
```
***
# `Docuemtn`
```java
// Check for document
        if (update.message() != null && update.message().document() != null && userStates.get(update.message().from().id()) == UserState.SHARING_DOCUMENT) {
            Long chatID = update.message().from().id();
            Document document = update.message().document();
            String fileId = document.fileId();
            String fileName = document.fileName() != null ? document.fileName() : "document_" + fileId;

            // Confirm document received
            String confirmationMessage = "Document received: " + fileName + "\nThank you for your submission.";
            bot.execute(new SendMessage(chatID, confirmationMessage));

            // Optional: Download the document
            try {
                GetFile getFile = new GetFile(fileId);
                GetFileResponse fileResponse = bot.execute(getFile);
                String filePath = fileResponse.file().filePath();
                String downloadUrl = "https://api.telegram.org/file/bot" + bot.getToken() + "/" + filePath;
                System.out.println("Document download URL: " + downloadUrl);
                // You can use an HTTP client (e.g., OkHttp) to download the file from downloadUrl
            } catch (Exception e) {
                bot.execute(new SendMessage(chatID, "Error processing the document."));
                e.printStackTrace();
            }

            // Update state (e.g., complete registration)
            userStates.put(chatID, null); // Or move to another state
            return;
        }
```
***
# `Audio`
```java
// Check for audio
        if (update.message() != null && update.message().audio() != null && userStates.get(update.message().from().id()) == UserState.SHARING_AUDIO) {
            Long chatID = update.message().from().id();
            Audio audio = update.message().audio();
            String fileId = audio.fileId();
            String fileName = audio.title() != null ? audio.title() : "audio_" + fileId;

            // Confirm audio received
            String confirmationMessage = "Audio received: " + fileName + "\nThank you for your submission.";
            bot.execute(new SendMessage(chatID, confirmationMessage));

            // Optional: Download the audio
            try {
                GetFile getFile = new GetFile(fileId);
                GetFileResponse fileResponse = bot.execute(getFile);
                String filePath = fileResponse.file().filePath();
                String downloadUrl = "https://api.telegram.org/file/bot" + bot.getToken() + "/" + filePath;
                System.out.println("Audio download URL: " + downloadUrl);
                // You can use an HTTP client (e.g., OkHttp) to download the file from downloadUrl
            } catch (Exception e) {
                bot.execute(new SendMessage(chatID, "Error processing the audio."));
                e.printStackTrace();
            }

            // Update state
            userStates.put(chatID, null);
            return;
        }
```
***
# Send `Photo`, `Document`, `Audio`
```java
  // Send photos
                if (photos != null && !photos.isEmpty()) {
                    for (String fileId : photos) {
                        bot.execute(new SendPhoto(chatID, fileId).caption("Photo"));
                    }
                    bot.execute(new SendMessage(chatID, "Sent " + photos.size() + " photo(s)."));
                }

                // Send document
                if (documentFileId != null) {
                    bot.execute(new SendDocument(chatID, documentFileId).caption("Document"));
                    bot.execute(new SendMessage(chatID, "Sent document."));
                }

                // Send audio
                if (audioFileId != null) {
                    bot.execute(new SendAudio(chatID, audioFileId).caption("Audio"));
                    bot.execute(new SendMessage(chatID, "Sent audio."));
                }
```
***
