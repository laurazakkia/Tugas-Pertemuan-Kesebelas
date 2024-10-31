# Konsep Pemrograman Berorientasi Objek - REPORT (UPLOAD .CSV)
---
Button Upload ini berfungsi untuk menambahkan data dari file .csv yang kita upload.
---
## Langkah - langkah : 
### 1. Menambahkan button Upload pada design yang sudah ada.
![image](https://github.com/user-attachments/assets/8c60000d-c64b-403d-9456-c6bb4e2815aa)

### 2. Menambahkan source code pada button Upload 
<pre>
  private void btnUploadActionPerformed(java.awt.event.ActionEvent evt) {                                          
        // TODO add your handling code here:
        JFileChooser jfc = new JFileChooser(FileSystemView.getFileSystemView().getHomeDirectory());
        int returnValue = jfc.showOpenDialog(null);
        if (returnValue == JFileChooser.APPROVE_OPTION) {
            File filePilihan = jfc.getSelectedFile();
            System.out.println("yang dipilih : " + filePilihan.getAbsolutePath());
            try {
                BufferedReader br = new BufferedReader(new FileReader(filePilihan));
                String baris = new String("");
                String pemisah = ";";
                while ((baris = br.readLine()) != null) //returns a Boolean value
                {
                    String[] dataMhs = baris.split(pemisah);
                    String kodeMK = dataMhs[0];
                    String sks = dataMhs[1];
                    String namaMK = dataMhs[2];
                    String semesterAjar = dataMhs[3];
                    if (!kodeMK.isEmpty() && !sks.isEmpty() && !namaMK.isEmpty() && !semesterAjar.isEmpty()) {
                        try {
                            Class.forName(driver);
                            conn = DriverManager.getConnection(koneksi, user, password);
                            conn.setAutoCommit(false);

                            String sql = "INSERT INTO matakuliah VALUES(?,?,?,?)";
                            pstmt = conn.prepareStatement(sql);

                            pstmt.setString(1, kodeMK);
                            pstmt.setLong(2, Long.parseLong(sks));
                            pstmt.setString(3, namaMK);
                            pstmt.setLong(4, Long.parseLong(semesterAjar));

                            pstmt.executeUpdate();
                            conn.commit();
                            pstmt.close();
                            conn.close();
                            bersih();
                            
                            showTable();
                        } catch (ClassNotFoundException | SQLException ex) {
                            JOptionPane.showMessageDialog(null, "Terjadi Kesalahan Saat Pengisian Data");
                        }
                    }
                }
            } catch (FileNotFoundException ex) {
                Logger.getLogger(mataKuliah.class.getName()).log(Level.SEVERE, null, ex);
            } catch (IOException ex) {
                Logger.getLogger(mataKuliah.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
    }     
</pre>

### 3. Membuat file excel dengan format (.csv)
![image](https://github.com/user-attachments/assets/f5284a20-f723-40a9-a704-45f950be5460)

## Implementasi : 
### 1. Pilih upload
![image](https://github.com/user-attachments/assets/e296490e-2c5d-43f9-b76a-5f39eaa78094)

### 2. Pilih file (.csv) yang sudah dibuat dan open file
![image](https://github.com/user-attachments/assets/d930c880-d360-46ac-a8f6-0a5b8f0b8262)

### 3. Data akan otomatis ditambahkan
![image](https://github.com/user-attachments/assets/105f267a-04a0-49b1-a5dc-0c5ebd61008a)



