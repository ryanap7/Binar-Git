# Penjelasan Kode React Native: Komponen Skeleton

Dalam konten ini, kita akan membahas kode dari komponen Skeleton dalam aplikasi React Native. Komponen ini digunakan untuk membuat efek kerangka kosong (skeleton) yang akan digunakan saat konten sedang dimuat.

```javascript
import React, { useEffect } from 'react';
import { Animated, Easing, StyleSheet, View } from 'react-native';
import LinearGradient from 'react-native-linear-gradient';

interface Props {
  children?: JSX.Element;
  height: any;
  width: any;
  borderRadius?: number;
  marginBottom?: number;
  marginRight?: number;
}
```

Di atas adalah bagian impor (import) dari modul yang diperlukan. Kode ini mengimpor berbagai komponen dan pustaka yang akan digunakan dalam komponen Skeleton.

```javascript
const Animation = Animated.createAnimatedComponent(LinearGradient);

const Skeleton = ({
  children,
  height,
  width,
  borderRadius,
  marginBottom,
  marginRight,
}: Props) => {
```

Pada bagian ini, kami mendefinisikan komponen Animation yang merupakan hasil dari penggunaan createAnimatedComponent untuk memungkinkan animasi pada komponen LinearGradient. Selanjutnya, kita mendefinisikan komponen Skeleton yang menerima beberapa prop yang akan digunakan untuk mengatur tampilan dari komponen ini.

```javascript
const animatedValue = new Animated.Value(0);

useEffect(() => {
  Animated.loop(
    Animated.timing(animatedValue, {
      toValue: 1,
      duration: 1000,
      easing: Easing.ease,
      useNativeDriver: true,
    }),
  ).start();
});
```

Pada bagian ini, kita menggunakan React Hooks useEffect untuk membuat animasi looping. Ini berarti animasi akan terus berjalan sepanjang waktu dengan menggunakan properti animatedValue yang akan bergerak dari 0 hingga 1 dalam waktu 1000 milidetik (1 detik). Penggunaan useNativeDriver: true mengoptimalkan kinerja animasi.

```javascript
const translateX = animatedValue.interpolate({
  inputRange: [0, 1],
  outputRange: [-width, width],
});
```

Di sini, kita menggunakan metode interpolate untuk menghasilkan nilai-nilai yang akan digunakan untuk menggerakkan efek kerangka. Nilai ini akan bergerak dari -width hingga width saat animasi berlangsung.

```javascript
return (
  <View
    style={[
      styles.container,
      {
        height,
        width,
        borderRadius,
        marginBottom,
        marginRight,
      },
    ]}>
    <Animation
      colors={['#a0a0a0', '#b0b0b0', '#b0b0b0', '#a0a0a0']}
      start={{ x: 0, y: 0 }}
      end={{ x: 1, y: 1 }}
      style={{
        ...StyleSheet.absoluteFillObject,
        transform: [{ translateX: translateX }],
      }}
    />
    {children}
  </View>
);
```

Di bagian ini, kita mengembalikan tampilan komponen Skeleton. Ini termasuk dalam <View> yang memiliki gaya (style) yang dapat disesuaikan dengan prop yang diberikan saat penggunaan komponen ini. Kemudian, kita memiliki elemen Animation yang akan menghasilkan efek kerangka dengan gradient warna tertentu. Transformasi menggunakan nilai translateX yang sudah dihitung sebelumnya. Terakhir, kita meletakkan properti children untuk mengizinkan konten tambahan yang akan muncul di atas efek kerangka.

```javascript
const styles = StyleSheet.create({
  container: {
    backgroundColor: '#a0a0a0',
    overflow: 'hidden',
  },
});
```

Di bagian ini, kita mendefinisikan gaya (style) untuk kontainer komponen Skeleton. Warna latar belakangnya diatur menjadi abu-abu (#a0a0a0) dan overflow (overflow: 'hidden') agar tidak ada bagian yang muncul di luar kontainer.

# Penutup

Komponen Skeleton ini digunakan untuk meningkatkan pengalaman pengguna saat menunggu konten dimuat dalam aplikasi React Native. Dengan efek kerangka, pengguna dapat melihat bahwa sesuatu sedang terjadi di latar belakang, yang membuat pengalaman mereka lebih baik.

