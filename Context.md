# Lychee Lock – Project Context

Lychee Lock is a secure, privacy-first password manager built with **Angular** and **Supabase**.

## 🔐 Core Features
- **Email/password login** via Supabase Auth
- **Client-side encryption** using the Web Crypto API
- **TOTP-based 2FA** (Google Authenticator-compatible)
- Vault data is encrypted in-browser and stored in Supabase
- No sensitive data is ever sent unencrypted

## 🧱 Database Structure (Supabase)

### `categories`
- `id UUID PRIMARY KEY`
- `user_id UUID REFERENCES auth.users(id)`
- `name TEXT`

### `passwords`
- `id UUID PRIMARY KEY`
- `user_id UUID REFERENCES auth.users(id)`
- `category_id UUID REFERENCES categories(id)`
- `title TEXT`
- `username TEXT`
- `password_encrypted TEXT`
- `url TEXT`
- `notes TEXT`
- `created_at TIMESTAMP`
- `updated_at TIMESTAMP`

## 🔐 Security Notes
- All passwords are encrypted client-side with AES-GCM
- Encryption key is derived from user's password using PBKDF2
- Each user gets a default `"Undefined"` category on signup
- Supabase RLS policies enforce per-user data access
- TOTP secret is generated client-side, encrypted, and stored in Supabase

## ⚙️ Tech Stack
- **Frontend**: Angular + TailwindCSS
- **Auth & DB**: Supabase
- **Crypto**: Web Crypto API
- **2FA**: otplib for TOTP (QR + 6-digit codes)
- **Hosting**: Vercel

## 🔄 Flow Summary
1. User signs up with email/password (Supabase Auth)
2. App generates TOTP secret + default category
3. User logs in, enters TOTP code, derives encryption key
4. Vault entries are fetched, decrypted, and shown