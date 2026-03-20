# telegram-drive-bot
import os
from pyrogram import Client, filters
from google.oauth2 import service_account
from googleapiclient.discovery import build
from googleapiclient.http import MediaFileUpload

api_id = 30566275
api_hash = "0be79f019d1253e2e5108b7d6b6e3f92"
bot_token = AAHmc0-jbrxsC9p61vH0_hbJmxyEhuvxfww

app = Client("bot", api_id=api_id, api_hash=api_hash, bot_token=bot_token)

SCOPES = ['https://www.googleapis.com/auth/drive']
creds = service_account.Credentials.from_service_account_file(
    'credentials.json', scopes=SCOPES)
    drive_service = build('drive', 'v3', credentials=creds)

    FOLDER_ID = "1RmxxQZ7OoLJFvNutZT_On82cqktQxu8O"

    def upload_to_drive(file_path, file_name):
        file_metadata = {'name': file_name, 'parents': [FOLDER_ID]}
            media = MediaFileUpload(file_path, resumable=True)

                file = drive_service.files().create(
                        body=file_metadata,
                                media_body=media,
                                        fields='id'
                                            ).execute()

                                                file_id = file.get('id')

                                                    drive_service.permissions().create(
                                                            fileId=file_id,
                                                                    body={'role': 'reader', 'type': 'anyone'}
                                                                        ).execute()

                                                                            return f"https://drive.google.com/uc?id={file_id}&export=download"

                                                                            @app.on_message(filters.document | filters.video | filters.photo)
                                                                            async def handle(client, message):

                                                                                msg = await message.reply_text("⏳ Processing...")

                                                                                    file = message.document or message.video or message.photo[-1]
                                                                                        file_name = getattr(file, "file_name", "file")

                                                                                            path = await message.download()

                                                                                                await msg.edit_text("⏳ Uploading to Drive...")

                                                                                                    link = upload_to_drive(path, file_name)

                                                                                                        os.remove(path)

                                                                                                            await msg.edit_text(f"✅ Download link:\n{link}")

                                                                                                            app.run()