# Files API

To interact with the **Files API**, all HTTP requests should be prefixed with `/api/files`.
For example,

```url
http://127.0.0.1:3000/api/files
```

1. Basic MFS interaction

- `GET /:cid` - Get file
- `POST /upload` - Upload file
- `POST /directory` - Create directory
- `PUT /list` - List directory files
- `PUT /copy` - Copy file or directory
- `POST /move` - Move file or directory
- `DELETE /remove` - Remove file
- ~~`PUT /pin/:cid` - Pin locally~~
- ~~`DELETE /pin/:cid` - Unpin locally~~
- `PUT /pin/pinata/:cid` - Pin remotely through Pinata
- `DELETE /pin/pinata/:cid` - Unpin remotely through Pinata

2. Bundled and Encrypted

- `POST /bundle/upload/file` - Upload file
- `POST /bundle/upload/folder` - Upload folder
- `POST /bundle/:cid` - Download file

---

# Usage

## Basic MFS Interaction

### 1. Get file

Downloads the file from its CID.

```
GET /:cid
```

#### Params

- cid `string` - CID of the file

#### Response

File stream of the file.

### 2. Upload file

Uploads a file to IPFS. This would simply upload the file to IPFS without any encryption.

```
POST /upload
```

#### Params

- file `File` - The file
- directory `string`- The MFS directory where the app will put the file.

#### Response

- cid `string` - The CID of the newly-uploaded file.

### 3. Create directory

Creates a new directory.

```
POST /directory
```

#### Params

- directory `string` - The MFS directory where the folder will be created.

#### Response

None

### 4. List files in directory

Lists the files present inside the directory.

```
PUT /list
```

#### Params

- directory `string` - The MFS directory.

#### Response

List of `FileEntry`. Each instance of `FileEntry` have the following attributes:

- name `string` - The name of the file
- type `"directory" | "file"` - Type of the entry.
- size `number` - The size of the file in bytes.
- cid `string` - The CID of the file
- ~~- is_pinned `boolean` - Whether the current file is pinned locally.~~
- is_pinned_pinata `boolean` - Whether the current file is pinned in Pinata.

### 5. Copy file or directory

Copies a file or folder.

```
PUT /copy
```

#### Params

- from `string` - The MFS path of the source file or folder
- to `string` - The MFS directory of the destination folder

#### Response

None

### 6. Move file or directory

Moves a file or folder.

```
POST /move
```

#### Params

- from `string` - The MFS path of the source file or folder
- to `string` - The MFS directory of the destination folder

#### Response

None

### 7. Remove file

Deletes a file from the local MFS. This won't unpin files from remote services.

```
DELETE /remove
```

#### Params

- directory `string` - The MFS path of the file or folder to be deleted

#### Response

None

### 8. Pin locally

**This is deprecated. Pinning locally is implicit using MFS**

Pins a file locally.

```
PUT /pin/:cid
```

#### Params

- cid `string` - The CID of the file to be pinned.

#### Response

None

### 9. Unpin locally

**This is deprecated. Pinning locally is implicit using MFS**

Unpins a file locally.

```
DELETE /pin/:cid
```

#### Params

- cid `string` - The CID of the file to be unpinned.

#### Response

None

### 10. Pin remotely through Pinata

Pins a file remotely through Pinata Service

```
PUT /pin/pinata/:cid
```

#### Params

- cid `string` - The CID of the file to be pinned.

#### Response

None

### 11. Unpin remotely through Pinata

Unpins a file pinned through Pinata Service

```
DELETE /pin/pinata/:cid
```

#### Params

- cid `string` - The CID of the file to be pinned.

#### Response

None

---

## Bundled and Encrypted Files

### 1. Upload file

Uploads a file to IPFS. This would bundle and encrypt the file first using `aes-256-gcm` scheme.

```
POST /bundle/upload/file
```

#### Params

- file `File` - The file to be uploaded
- directory `string`- The MFS directory where the app will put the file.
- passphrase `string`- The passphrase used to encrypt the file.
- willSaveKey `boolean` - Indicates whether the passphrase will be saved

#### Response

- cid `string` - The CID of the newly-uploaded file.

### 2. Upload folder

Uploads a folder to IPFS. This would bundle and encrypt the folder first using `aes-256-gcm` scheme.

```
POST /bundle/upload/folder
```

#### Params

- files `File[]` - List of files to be uploaded
- name `string`- The name of the folder.
- directory `string`- The MFS directory where the app will put the bundled files.
- passphrase `string`- The passphrase used to encrypt the folder.
- willSaveKey `boolean` - Indicates whether the passphrase will be saved

#### Response

- cid `string` - The CID of the newly-uploaded file.

### 3. Download file

Downloads and decrypts an encrypted file from IPFS. This would unbundle and decrypt the file first using `aes-256-gcm` scheme.

```
POST /bundle/:cid
```

#### Params

- cid `string` - The CID of the bundled file.
- filename `string` - Desired filename of the downloaded file.
- passphrase `string`- The passphrase used to decrypt the folder. This is optional if the passphrase was saved locally.

#### Response

If the bundle contains only one file, then this will return that file.
If the bundle contains a folder, then this will return a zip file representing that folder.
