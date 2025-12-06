## Choosing a Computer for Open Source Medical Imaging Projects

*A practical guide for researchers handling large datasets (CT scans, MRIs, X-rays)*

---

### TL;DR

| Decision | Recommendation |
|----------|----------------|
| Form factor | Mini PC (unless you need internal drive bays) |
| CPU | Intel N100/N150 is sufficient for transfers; i5+ for processing |
| RAM | 16GB minimum |
| USB | USB 3.2 Gen2 (10Gbps) ports preferred |
| OS | Linux (Ubuntu LTS) |
| Filesystem | ext4 for Linux-only; exFAT for cross-platform |

---

### Why Mini PC Works for Most Cases

Medical imaging data transfer is **storage-limited, not CPU-limited**:

- Typical HDD: 150-180 MB/s
- USB 3.0: 400+ MB/s capacity
- CPU usage during file copy: <5%

A $150-200 mini PC transfers data **as fast as** a $1000 workstation.

---

### When to Choose Desktop Instead

- Shucking drives (removing HDDs from enclosures)
- Running 4+ drives simultaneously via internal SATA
- Building a PACS server or local XNAT instance
- GPU processing (AI inference, 3D reconstruction)

---

### Linux is the Right Choice

**rsync is essential for medical data:**

```bash
# Resume-able, verifiable transfers for 100GB+ DICOM folders
rsync -avh --progress /source/study/ /destination/study/
```

Benefits:
- Resumes interrupted transfers
- Verifies integrity (critical for research data)
- Scriptable for batch processing
- Lower resource usage than Windows
- Free (no license cost for grant budgets)

---

### Filesystem Selection

| Scenario | Format | Why |
|----------|--------|-----|
| Drives stay with Linux workstation | ext4 | Fastest, journaled, safe |
| Sharing with collaborators (Mac/Win) | exFAT | Universal compatibility |
| HIPAA/clinical environments | ext4 + encryption | Security compliance |

**Always unmount before disconnecting.** Medical data is irreplaceable.

---

### Recommended Specs

**Minimum (data transfer only):**
- Intel N100, 16GB RAM, 256GB SSD
- 4× USB 3.0+ ports
- ~$150-200

**Recommended (light processing):**
- Intel i5/Ryzen 5, 32GB RAM, 512GB NVMe
- USB 3.2 Gen2, 2.5GbE Ethernet
- ~$400-600


---

### Essential Tools

```bash
# Install on Ubuntu
sudo apt install rsync dcm2niix exfatprogs

# DICOM to NIfTI conversion
dcm2niix -o /output -f "%p_%s" /input/dicom/

# Verify transfer integrity
md5sum /source/file > checksum.md5
md5sum -c checksum.md5
```

---

### Key Lessons

1. **Don't overspend** — file transfer doesn't need powerful hardware
2. **Prioritize USB ports** — more ports = more simultaneous drives
3. **Use rsync** — resume capability saves hours on failed transfers
4. **Journal your filesystem** — ext4 protects against power loss
5. **Script everything** — reproducibility matters in research

---

*Last updated: December 2024*
*Context: PanCanAID, HALAJ, and similar multi-center imaging studies*