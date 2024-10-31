![image](https://github.com/user-attachments/assets/07e0eb65-3879-4f3a-bd9d-5cbd605bec25)# Konsep Pemrograman Berorientasi Objek - REPORT (UPLOAD .CSV)
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

