# Export DTOs

> `routes/export/schemas.ts`

## PDFExportResponse

```typescript
const PDFExportResponse = z.object({
  download_url: z.string().url(),
  expires_at: z.string(),
  size_bytes: z.number(),
});
```

## ShareLinkResponse

```typescript
const ShareLinkResponse = z.object({
  url: z.string().url(),
  slug: z.string(),
  og_title: z.string(),
  og_description: z.string(),
  og_image_url: z.string().url(),
});
```
