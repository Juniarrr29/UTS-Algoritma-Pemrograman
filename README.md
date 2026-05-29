#include <iostream>
#include <fstream> 

using namespace std;
struct Tugas {
    char namaTugas[50];
    char mataKuliah[50];
    char deadline[20];
    bool isDone;
};
class AplikasiTaskku {
private:
    char correctUsername[30];
    char correctPassword[20];
    Tugas daftarTugas[100];
    int jumlahTugas;
    int batas; 
    bool cekSama(char input[], char asli[]) {
        int i = 0;
        while (input[i] != '\0' || asli[i] != '\0') {
            if (input[i] != asli[i]) {
                return false; 
            }
            i++;
        }
        return true;
    }

public:
    AplikasiTaskku() {
        jumlahTugas = 0;
        batas = 3; 
        char userAsli[] = "Fardan Arib Juniar";
        char passAsli[] = "2500018136";
        
        int i = 0;
        while(userAsli[i] != '\0') { correctUsername[i] = userAsli[i]; i++; }
        correctUsername[i] = '\0';
        
        int j = 0;
        while(passAsli[j] != '\0') { correctPassword[j] = passAsli[j]; j++; }
        correctPassword[j] = '\0';
    }

    bool login() {
        char username[30];
        char password[20];
        int interracial = 0; 
        int percobaan = 0;

        cout << "=========================================\n";
        cout << "       SISTEM LOGIN UTAMA TASKKU         \n";
        cout << "=========================================\n";

        while (percobaan < batas) {
            cout << "Masukkan Username (Nama) : ";
            cin.getline(username, 30);
            cout << "Masukkan Password (NIM)  : ";
            cin.getline(password, 20);

            if (cekSama(username, correctUsername) && cekSama(password, correctPassword)) {
                cout << "\n>> LOGIN BERHASIL! Selamat Datang, " << correctUsername << ".\n";
                return true;
            } else {
                percobaan++; 
                cout << ">> Username atau Password salah!\n";
                if (percobaan < batas) {
                    cout << ">> Sisa percobaan: " << (batas - percobaan) << " kali lagi.\n\n";
                }
            }
        }
        cout << "\n>> LOGIN GAGAL! Batas percobaan login habis.\n";
        return false;
    }
    void tambahTugas() {
        if (jumlahTugas >= 100) {
            cout << "Daftar tugas penuh!\n";
            return;
        }

        cout << "\n--- TAMBAH TUGAS BARU ---\n";
        
        cout << "Nama Tugas  : ";
        cin.getline(daftarTugas[jumlahTugas].namaTugas, 50);
        
        cout << "Mata Kuliah : ";
        cin.getline(daftarTugas[jumlahTugas].mataKuliah, 50);
        
        cout << "Deadline    : ";
        cin.getline(daftarTugas[jumlahTugas].deadline, 20);
        
        daftarTugas[jumlahTugas].isDone = false; 
        jumlahTugas++;
        
        cout << ">> Tugas berhasil ditambahkan!\n";
    }

    void lihatTugasAktif() {
        cout << "\n--- DAFTAR TUGAS AKTIF (BELUM SELESAI) ---\n";
        bool ditemukan = false;

        for (int i = 0; i < jumlahTugas; i++) {
            if (!daftarTugas[i].isDone) {
                cout << "ID Tugas    : " << i << "\n";
                cout << "Nama Tugas  : " << daftarTugas[i].namaTugas << "\n";
                cout << "Mata Kuliah : " << daftarTugas[i].mataKuliah << "\n";
                cout << "Deadline    : " << daftarTugas[i].deadline << "\n";
                cout << "-----------------------------------------\n";
                ditemukan = true;
            }
        }
        if (!ditemukan) {
            cout << "Hebat! Tidak ada tugas aktif yang tersisa.\n";
        }
    }

    void tandaiSelesai() {
        lihatTugasAktif();
        if (jumlahTugas == 0) return;

        int id;
        cout << "Masukkan ID Tugas yang sudah selesai (done): ";
        cin >> id;
        cin.ignore();

        if (id >= 0 && id < jumlahTugas && !daftarTugas[id].isDone) {
            daftarTugas[id].isDone = true;
            cout << ">> Sukses! Tugas berhasil ditandai selesai.\n";
        } else {
            cout << ">> ID Tugas tidak valid atau sudah selesai!\n";
        }
    }

    void lihatRiwayatSelesai() {
        cout << "\n--- RIWAYAT TUGAS YANG SUDAH SELESAI ---\n";
        bool ditemukan = false;

        for (int i = 0; i < jumlahTugas; i++) {
            if (daftarTugas[i].isDone) {
                cout << "Nama Tugas  : " << daftarTugas[i].namaTugas << "\n";
                cout << "Mata Kuliah : " << daftarTugas[i].mataKuliah << "\n";
                cout << "Deadline    : " << daftarTugas[i].deadline << "\n";
                cout << "Status      : SELESAI (DONE)\n";
                cout << "-----------------------------------------\n";
                ditemukan = true;
            }
        }
        if (!ditemukan) {
            cout << "Belum ada tugas yang selesai dikerjakan.\n";
        }
    }

    void hapusTugas() {
        cout << "\n--- HAPUS TUGAS ---\n";
        for (int i = 0; i < jumlahTugas; i++) {
            cout << "ID: " << i << " | Tugas: " << daftarTugas[i].namaTugas << " [" << (daftarTugas[i].isDone ? "Done" : "Aktif") << "]\n";
        }

        int id;
        cout << "Masukkan ID Tugas yang ingin dihapus permanen: ";
        cin >> id;
        cin.ignore();

        if (id >= 0 && id < jumlahTugas) {
            for (int j = id; j < jumlahTugas - 1; j++) {
                daftarTugas[j] = daftarTugas[j + 1];
            }
            jumlahTugas--; 
            cout << ">> Tugas berhasil dihapus dari sistem.\n";
        } else {
            cout << ">> ID tidak ditemukan!\n";
        }
    }

    void simpanKeFile() {
        ofstream fileArsip("arsip_tugas.txt");

        if (fileArsip) {
            fileArsip << "=== ARSIP DATA TUGAS FARDAN (TASKKU) ===\n\n";
            for (int i = 0; i < jumlahTugas; i++) {
                fileArsip << "Nama Tugas  : " << daftarTugas[i].namaTugas << "\n";
                fileArsip << "Mata Kuliah : " << daftarTugas[i].mataKuliah << "\n";
                fileArsip << "Deadline    : " << daftarTugas[i].deadline << "\n";
                fileArsip << "Status      : " << (daftarTugas[i].isDone ? "SELESAI (DONE)" : "AKTIF (BELUM SELESAI)") << "\n";
                fileArsip << "-------------------------------------\n";
            }
            fileArsip.close();
            cout << ">> Berhasil! Seluruh tugas telah diarsipkan ke dalam file 'arsip_tugas.txt'.\n";
        } else {
            cout << ">> Gagal mengaktifkan sistem arsip file eksternal.\n";
        }
    }
};
int main() {
    AplikasiTaskku taskku;

    if (!taskku.login()) {
        return 0; 
    }

    int pilihan;
    do {
        cout << "\n=========================================\n";
        cout << "                 MENU UTAMA       \n";
        cout << "=========================================\n";
        cout << "1. Tambah Tugas Baru\n";
        cout << "2. Lihat Daftar Tugas Aktif\n";
        cout << "3. Tandai Tugas Selesai (Done)\n";
        cout << "4. Lihat Riwayat Tugas Selesai\n";
        cout << "5. Hapus Tugas\n";
        cout << "6. Simpan Seluruh Tugas ke File (.txt)\n";
        cout << "7. Keluar Aplikasi\n";
        cout << "Pilih Menu (1-7): ";
        cin >> pilihan;

        cin.ignore(); 

        switch (pilihan) {
            case 1: taskku.tambahTugas(); break;
            case 2: taskku.lihatTugasAktif(); break;
            case 3: taskku.tandaiSelesai(); break;
            case 4: taskku.lihatRiwayatSelesai(); break;
            case 5: taskku.hapusTugas(); break;
            case 6: taskku.simpanKeFile(); break;
            case 7: cout << "Keluar program!\n"; break;
            default: cout << "Pilihan tidak valid! Masukkan angka 1 sampai 7.\n"; break;
        }
    } while (pilihan != 7);

    return 0;
}
