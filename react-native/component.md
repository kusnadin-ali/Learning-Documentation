# 📱 Dokumentasi Belajar: React Native Components
 
> **Catatan Belajar Pribadi** | Dibuat: Maret 2026  
> Topik: Memahami dasar-dasar Component di React Native  
> Source :    
> -  [React Native Full Course 2026 | Build a Mobile App Using Expo](https://www.youtube.com/watch?v=RdJhqaOIWn0)
---
## 📚 Daftar Isi
 
1. [Apa itu Component?](#apa-itu-component)
2. [Jenis-Jenis Component](#jenis-jenis-component)
 
---
## Apa itu Component?

Component adalah **blok bangunan utama** di React Native. Bayangkan seperti "lego" — setiap bagian UI (tombol, teks, gambar) adalah sebuah component yang bisa digabungkan.

---
## Jenis Jenis Component
Dalam React native terdapat component penting pembentuk view nya. berikut merupakan component penting yang harus di mengerti.
1. [Stack](#stack-component)
2. [Tabs](#tabs-component)
3. [View](#view-component)
4. [Text](#text-component)
5. [TextInput](#text-input-component)
6. [ActivityIndicator](#activity-indicator)
7. [Button](#button-component)
___
### **1. Stack Component**
**Stack** merupakan component yang ditaruh di file `_layout.tsx`. di file tersebut bertujuan untuk mengatur layout tampilan view yang ingin di tampilkan, salah satunya **Stack Component** ditempatkan disitu. berikut contoh bentuk Stack Component.
![Home](https://img.sanishtech.com/u/636e16388a0c5940eaad5aa84a0ddf95.png)
![About: after click Go To About](https://img.sanishtech.com/u/e3b0c2290f087ccae95b93c93e760a3d.png).
    
    
bentuk code pada `_layout.tsx` :
```javascript
import { Stack } from "expo-router";
export default function RootLayout() {
  return (
    <Stack
      screenOptions={{
        headerStyle: { backgroundColor: "cornflowerblue" },
        headerTintColor: "white",
        animation: "slide_from_right",
      }}
    >
      <Stack.Screen name="index" options={{ title: "Home" }} />
      <Stack.Screen name="about" options={{ title: "About" }} />
    </Stack>
  );
}
```
`Stack.Screen` berfungsi untuk mendefinisikan terdapat apa aja screen pada layout tersebut. Jika kita mendefinisikan pada `Stack.Screen`, kita dapat memanggil sebagai route yang telah di definisikan. contoh seperti berikut
```javascript
export default function Index() {
  const router = useRouter();
  return (
    <View style={styles.container}>
      <Text style={styles.helloWorldTitle}>Hello World</Text>
      <TextInput placeholder="Email"/>
      <ActivityIndicator size={"large"}></ActivityIndicator>
      {/* <Link href={"/about"}>About</Link> */}
      <Button title="Go to about" onPress={() => router.push("/about")}></Button>
    </View>
  );
}
```
Pada code tersebut kita dapat menggunakan router.push setelah di definisikan di `Stack.screen`.
### **2. Tabs Component**
Selanjutnya merupakan `Tabs Component`, yang mempunyai bentuk seperti Tab yang memiliki bentuk seperti di gambar dibawah ini.
![contoh tab component](https://img.sanishtech.com/u/b849224d165432dee311e8e793e27bce.png)
mekanisme `Tabs` ini sama seperti `Stack` tetapi dalam bentuk `Tabs` dan yang paling keliatan itu terdapat button `Tabs` di bawah layar dan tidak ada tombol back seperti `Stack`

---

### **3. View Component**

**View** adalah container utama di React Native — fungsinya seperti `<div>` di HTML. Semua elemen UI harus dibungkus di dalam `View`.

> 💡 View sendiri tidak terlihat secara visual, ia hanya berfungsi sebagai "kotak" untuk mengatur posisi dan layout elemen di dalamnya.

**Penggunaan dasar:**
```javascript
import { View } from "react-native";

export default function Index() {
  return (
    <View style={{ flex: 1, backgroundColor: "#fff", padding: 20 }}>
      {/* Taruh component lain di sini */}
    </View>
  );
}
```

**Props yang sering dipakai:**

| Props | Fungsi |
|-------|--------|
| `style` | Mengatur tampilan (ukuran, warna, padding, dll.) |
| `onLayout` | Dipanggil saat ukuran View berubah |

---

### **4. Text Component**

**Text** digunakan untuk menampilkan tulisan. Di React Native, **semua teks wajib dibungkus dengan `<Text>`** — tidak boleh ditaruh langsung di dalam `<View>`.

**Penggunaan dasar:**
```javascript
import { View, Text } from "react-native";

export default function Index() {
  return (
    <View>
      <Text>Teks biasa</Text>
      <Text style={{ fontSize: 24, fontWeight: "bold", color: "navy" }}>
        Teks besar dan tebal
      </Text>
      <Text numberOfLines={2}>
        Teks ini akan terpotong jika melebihi 2 baris dan sisanya
        akan diganti dengan titik-titik...
      </Text>
    </View>
  );
}
```

**Props yang sering dipakai:**

| Props | Fungsi |
|-------|--------|
| `style` | Mengatur font, warna, ukuran teks |
| `numberOfLines` | Batas maksimal baris teks yang tampil |
| `onPress` | Aksi saat teks ditekan |

---

### **5. TextInput Component**

**TextInput** adalah kolom input untuk menerima teks dari pengguna, seperti form login, kolom pencarian, dan sebagainya.

**Penggunaan dasar:**
```javascript
import { useState } from "react";
import { View, TextInput, Text } from "react-native";

export default function Index() {
  const [email, setEmail] = useState("");

  return (
    <View>
      <TextInput
        placeholder="Masukkan email..."
        value={email}
        onChangeText={(text) => setEmail(text)}
        style={{
          borderWidth: 1,
          borderColor: "#ccc",
          borderRadius: 8,
          padding: 10,
        }}
      />
      <Text>Email kamu: {email}</Text>
    </View>
  );
}
```

**Contoh TextInput untuk password:**
```javascript
<TextInput
  placeholder="Password"
  secureTextEntry={true}  // teks jadi bintang-bintang
  style={{ borderWidth: 1, padding: 10 }}
/>
```

**Props yang sering dipakai:**

| Props | Fungsi |
|-------|--------|
| `placeholder` | Teks abu-abu sebelum user mengetik |
| `value` | Nilai teks saat ini (dikontrol lewat state) |
| `onChangeText` | Fungsi yang dipanggil setiap teks berubah |
| `secureTextEntry` | Menyembunyikan teks (untuk password) |
| `keyboardType` | Jenis keyboard: `"email-address"`, `"numeric"`, dll. |
| `multiline` | Mengizinkan teks lebih dari satu baris |

---

### **6. ActivityIndicator**

**ActivityIndicator** adalah animasi loading berputar (spinner) yang menandakan ada proses yang sedang berjalan, seperti mengambil data dari API.

**Penggunaan dasar:**
```javascript
import { View, ActivityIndicator } from "react-native";

export default function Index() {
  return (
    <View>
      {/* Loading kecil */}
      <ActivityIndicator />

      {/* Loading besar dengan warna custom */}
      <ActivityIndicator size="large" color="cornflowerblue" />
    </View>
  );
}
```

**Contoh penggunaan nyata — tampilkan saat data sedang dimuat:**
```javascript
import { useState } from "react";
import { View, ActivityIndicator, Text } from "react-native";

export default function Index() {
  const [isLoading, setIsLoading] = useState(true);

  return (
    <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
      {isLoading ? (
        <ActivityIndicator size="large" color="cornflowerblue" />
      ) : (
        <Text>Data sudah dimuat!</Text>
      )}
    </View>
  );
}
```

**Props yang sering dipakai:**

| Props | Fungsi |
|-------|--------|
| `size` | Ukuran spinner: `"small"` atau `"large"` |
| `color` | Warna spinner |
| `animating` | `true/false` untuk tampilkan atau sembunyikan (default: `true`) |

---

### **7. Button Component**

**Button** adalah tombol siap pakai bawaan React Native. Tampilannya sederhana dan mengikuti style default sistem (iOS/Android). Untuk tampilan yang lebih custom, biasanya digunakan `TouchableOpacity` sebagai gantinya.

**Penggunaan dasar:**
```javascript
import { View, Button } from "react-native";

export default function Index() {
  return (
    <View>
      <Button
        title="Tekan Saya"
        onPress={() => alert("Tombol ditekan!")}
      />

      {/* Tombol dengan warna berbeda */}
      <Button
        title="Hapus"
        color="red"
        onPress={() => console.log("dihapus")}
      />

      {/* Tombol yang dinonaktifkan */}
      <Button
        title="Tidak Aktif"
        disabled={true}
        onPress={() => {}}
      />
    </View>
  );
}
```

**Contoh dengan navigasi (seperti di Stack sebelumnya):**
```javascript
import { useRouter } from "expo-router";
import { View, Button } from "react-native";

export default function Index() {
  const router = useRouter();
  return (
    <View>
      <Button
        title="Pergi ke About"
        onPress={() => router.push("/about")}
      />
    </View>
  );
}
```

**Props yang sering dipakai:**

| Props | Fungsi |
|-------|--------|
| `title` | Teks yang tampil di tombol *(wajib)* |
| `onPress` | Fungsi yang dijalankan saat tombol ditekan *(wajib)* |
| `color` | Warna tombol |
| `disabled` | `true` untuk menonaktifkan tombol |

> ⚠️ **Catatan:** `Button` di React Native tidak bisa di-styling sepenuhnya (misal border radius, padding). Kalau butuh tombol yang lebih custom, gunakan **`TouchableOpacity`** atau **`Pressable`** sebagai gantinya.

---

*Dokumen ini dibuat untuk catatan belajar pribadi. Terus diupdate seiring belajar! 🚀*
