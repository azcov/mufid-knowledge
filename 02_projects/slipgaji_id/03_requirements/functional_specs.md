---
tags: [requirements, specs]
---
# Functional Specs — SlipGaji

## Core Features

### Employee Management
- Add employee: name, jabatan, salary structure (gaji pokok + all components)
- Configure once → reuse every payroll period

### Indonesian Salary Components
- Gaji pokok
- Tunjangan tetap (makan, transport, jabatan)
- Tunjangan tidak tetap (overtime, bonus)
- BPJS Kesehatan: 1% karyawan, 4% perusahaan
- BPJS Ketenagakerjaan: JHT (2% k + 3.7% p), JP (1% k + 2% p), JKK (0.24-1.74% p), JKM (0.3% p)
- PPh 21 TER (2024 method): tax calculated using Tarif Efektif Rata-rata
- All components visible on PDF — fully auditable

### Payslip Generation
- Select payroll period → click Generate → all PDFs created
- One-click for all employees (bulk)
- PDF: professional layout, company logo, full breakdown

### Salary History
- View all past payslips per employee
- Search by employee, period

### Export
- Growth+: Excel rekap gaji per period (all employees in one sheet)
- PDF download per employee or bulk ZIP

## User Flow
1. Setup company (name, logo, NPWP)
2. Add employees with salary structure
3. Each payroll period: select month → click Generate → download PDFs
4. Send via WhatsApp or email to employees
