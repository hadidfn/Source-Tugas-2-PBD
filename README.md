import java.util.Scanner;
import java.util.ArrayList;

class Menu {
    String nama;
    double harga;
    String kategori;

    Menu(String nama, double harga, String kategori) {
        this.nama = nama;
        this.harga = harga;
        this.kategori = kategori;
    }
}

import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    static Scanner input = new Scanner(System.in);
    static ArrayList<Menu> daftarMenu = new ArrayList<>();

    public static void main(String[] args) {
        
        daftarMenu.add(new Menu("Nasi Goreng", 20000, "Makanan"));
        daftarMenu.add(new Menu("Bubur Ayam", 15000, "Makanan"));
        daftarMenu.add(new Menu("Mie Rebus", 10000, "Makanan"));
        daftarMenu.add(new Menu("Es Teh", 5000, "Minuman"));
        daftarMenu.add(new Menu("Kopi", 5000, "Minuman"));
        daftarMenu.add(new Menu("Es Jeruk", 5000, "Minuman"));
        daftarMenu.add(new Menu("Es Kelapa Muda", 15000, "Minuman"));

        int pilihan;
        do {
            System.out.println("\n=== SISTEM RESTORAN ===");
            System.out.println("1. Menu Pelanggan");
            System.out.println("2. Kelola Menu Restoran");
            System.out.println("0. Keluar");
            System.out.print("Pilih menu: ");
            pilihan = input.nextInt();
            input.nextLine();

            switch (pilihan) {
                case 1 -> menuPelanggan();
                case 2 -> kelolaMenu();
                case 0 -> System.out.println("Terima kasih!");
                default -> System.out.println("Pilihan tidak valid.");
            }
        } while (pilihan != 0);
    }

    
    static void menuPelanggan() {
        System.out.println("\n=== DAFTAR MENU ===");
        tampilkanMenu();

        ArrayList<String> pesanan = new ArrayList<>();
        while (true) {
            System.out.print("\nMasukkan pesanan Anda (contoh : Kopi = 1) ketik 'selesai' jika sudah sudah : ");
            String inputPesanan = input.nextLine().trim();
            if (inputPesanan.equalsIgnoreCase("selesai")) break;

            if (!inputValid(inputPesanan)) {
                System.out.println(" Menu tidak ditemukan! Cek menu yang tersedia.");
                continue;
            }
            pesanan.add(inputPesanan);
        }

        cetakStruk(pesanan);
    }

    
    static void cetakStruk(ArrayList<String> pesanan) {
        double total = 0;

        System.out.println("\n=== STRUK PEMBAYARAN ===");
        for (String p : pesanan) {
            String[] parts = p.split("=");
            String nama = parts[0].trim();
            int jumlah = Integer.parseInt(parts[1].trim());

            for (Menu m : daftarMenu) {
                if (nama.equalsIgnoreCase(m.nama)) {
                    double totalItem = m.harga * jumlah;
                    System.out.printf("%-20s x%d  Rp %.0f%n", m.nama, jumlah, totalItem);
                    total += totalItem;
                }
            }
        }

        double pajak = total * 0.10;
        double pelayanan = 20000;
        double totalBayar = total + pajak + pelayanan;

        System.out.println("-----------------------------");
        System.out.printf("Total Pesanan      : Rp %.0f%n", total);
        System.out.printf("Pajak (10%%)         : Rp %.0f%n", pajak);
        System.out.printf("Biaya Pelayanan    : Rp %.0f%n", pelayanan);

        if (total > 100000) {
            double diskon = totalBayar * 0.10;
            totalBayar -= diskon;
            System.out.printf("Diskon 10%%          : Rp %.0f%n", diskon);
            System.out.println(">> Selamat! Anda mendapat diskon 10%");
        } else if (total > 50000) {
            System.out.println(">> Promo B1G1 untuk minuman tertentu");
        } else {
            System.out.println(">> Tidak ada promo");
        }

        System.out.println("-----------------------------");
        System.out.printf("Total Bayar        : Rp %.0f%n", totalBayar);
        System.out.println("Terima kasih telah berkunjung!");
    }

    
    static boolean inputValid(String pesanan) {
        String[] parts = pesanan.split("=");
        if (parts.length != 2) return false;
        String nama = parts[0].trim();
        try {
            Integer.parseInt(parts[1].trim());
        } catch (Exception e) {
            return false;
        }
        for (Menu m : daftarMenu) {
            if (nama.equalsIgnoreCase(m.nama)) return true;
        }
        return false;
    }

    
    static void kelolaMenu() {
        int pilih;
        do {
            System.out.println("\n=== KELOLA MENU RESTORAN ===");
            System.out.println("1. Tambah Menu");
            System.out.println("2. Ubah Harga Menu");
            System.out.println("3. Hapus Menu");
            System.out.println("4. Lihat Menu");
            System.out.println("0. Kembali");
            System.out.print("Pilih: ");
            pilih = input.nextInt();
            input.nextLine();

            switch (pilih) {
                case 1 -> tambahMenu();
                case 2 -> ubahHarga();
                case 3 -> hapusMenu();
                case 4 -> tampilkanMenu();
                case 0 -> {}
                default -> System.out.println("Pilihan tidak tersedia");
            }
        } while (pilih != 0);
    }

    static void tampilkanMenu() {
        for (int i = 0; i < daftarMenu.size(); i++) {
            Menu m = daftarMenu.get(i);
            System.out.printf("%d. %-20s Rp %.0f (%s)%n", i + 1, m.nama, m.harga, m.kategori);
        }
    }

    static void tambahMenu() {
        System.out.print("Nama menu baru: ");
        String nama = input.nextLine();
        System.out.print("Harga: ");
        double harga = input.nextDouble();
        input.nextLine();
        System.out.print("Kategori (Makanan/Minuman): ");
        String kategori = input.nextLine();

        daftarMenu.add(new Menu(nama, harga, kategori));
        System.out.println("Menu berhasil ditambahkan!");
    }

    static void ubahHarga() {
        tampilkanMenu();
        System.out.print("Masukkan nomor menu yang ingin diubah: ");
        int no = input.nextInt();
        input.nextLine();
        if (no < 1 || no > daftarMenu.size()) {
            System.out.println("Nomor tidak tersedia.");
            return;
        }
        System.out.print("Apakah Anda yakin ingin mengubah harga? (Ya/Tidak): ");
        String konfirmasi = input.nextLine();
        if (konfirmasi.equalsIgnoreCase("Ya")) {
            System.out.print("Masukkan harga baru: ");
            double hargaBaru = input.nextDouble();
            input.nextLine();
            daftarMenu.get(no - 1).harga = hargaBaru;
            System.out.println("Harga berhasil diubah!");
        } else {
            System.out.println("Dibatalkan.");
        }
    }

    static void hapusMenu() {
        tampilkanMenu();
        System.out.print("Masukkan nomor menu yang ingin dihapus: ");
        int no = input.nextInt();
        input.nextLine();
        if (no < 1 || no > daftarMenu.size()) {
            System.out.println("Nomor tidak valid.");
            return;
        }
        System.out.print("Apakah Anda yakin ingin menghapus menu ini? (Ya/Tidak): ");
        String konfirmasi = input.nextLine();
        if (konfirmasi.equalsIgnoreCase("Ya")) {
            daftarMenu.remove(no - 1);
            System.out.println("Menu berhasil dihapus!");
        } else {
            System.out.println("Dibatalkan.");
        }
    }
}
