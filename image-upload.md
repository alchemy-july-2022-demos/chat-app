# Image upload

## File Input

Here is an example file upload input for images:

```html
<input
  type="file"
  name="avatar"
  placeholder="upload avatar"
  accept="image/*"
  required
/>
```

## Preview Image

You can show a preview prior to submitting the form:

```js
const fileInput = form.querySelector("input[type=file]");
// you need an <img> to show the preview
const preview = form.querySelector("img");

fileInput.addEventListener("change", () => {
  const [file] = fileInput.files;
  preview.src = URL.createObjectURL(file);
});
```

## Submission

The file type is handled the same way as any other form data:

```js
const formData = new FormData(form);
const imageFile = formData.get("avatar");
```

You'll want to create a unique image name that includes the folder path. This might include the users id or other relevant id.

```js
// we don't want two file names to collide on supabase!
// so in this case, we can use familyName, but we want to
// keep the file extension the same (e.g. .png)
// const ext = imageFile.type.split('/')[1];
// const imageName = `${familyName}-avatar.${ext}`;

const imageName = `${user.id}/${imageFile.name}`;
```

## Supabase Client Upload

Here is an example method for uploading images:

```js
import { client, SUPABASE_URL } from "./client.js";

export async function uploadFamilyAvatar(imageName, imageFile) {
  // we can use the storage bucket to upload the image,
  // then use it to get the public URL
  const bucket = client.storage.from("avatars");

  const { data, error } = await bucket.upload(imageName, imageFile, {
    cacheControl: "3600",
    // in this case, we will _replace_ any
    // existing file with same name.
    upsert: true,
  });

  if (error) {
    // eslint-disable-next-line no-console
    console.log(error);
    return null;
  }

  // bug in supabase makes this return wrong value :(
  // const url = bucket.getPublicUrl(data.Key);

  // so we will make it ourselves.
  // note that we exported the url from `./client.js`
  const url = `${SUPABASE_URL}/storage/v1/object/public/${data.Key}`;

  return url;
}
```
