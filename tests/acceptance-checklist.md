# Acceptance Checklist

## Deployment

- [ ] GitHub repository is clean
- [ ] Railway configuration is valid
- [ ] Docker image tag is pinned
- [ ] Railway deployment succeeds
- [ ] Service reaches healthy state

## Security

- [ ] No hard-coded credentials
- [ ] Login is enabled
- [ ] Initial password is generated or supplied securely
- [ ] Secrets are not committed
- [ ] Public access is intentional

## Persistence

- [ ] Volume is attached at `/configs`
- [ ] Settings survive restart
- [ ] Settings survive redeploy
- [ ] Backup process is documented

## PDF

- [ ] Merge
- [ ] Split
- [ ] Compress
- [ ] Rotate
- [ ] Watermark
- [ ] Metadata
- [ ] Page extraction

## OCR

- [ ] English OCR
- [ ] Second supported language, if required by target users

## Office

- [ ] DOCX to PDF
- [ ] XLSX to PDF
- [ ] PPTX to PDF

## API

- [ ] Swagger/OpenAPI loads
- [ ] API key can be created
- [ ] Representative API request works
- [ ] Invalid API key is rejected

## Reliability

- [ ] Restart tested
- [ ] Redeploy tested
- [ ] Health check tested
- [ ] Logs inspected
- [ ] Failure behavior understood

## Human Review

- [ ] Licensing reviewed before Marketplace publication
- [ ] Marketplace publication explicitly authorized
