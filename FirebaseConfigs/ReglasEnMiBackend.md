rules_version = '2';
service cloud.firestore {
match /databases/{database}/documents {
match /chat/{doc} {
allow read, create : if request.auth != null
}
}
}
