```javascript
import React, { useEffect } from 'react';
import { Animated, Easing, StyleSheet, View } from 'react-native';
import LinearGradient from 'react-native-linear-gradient';

// Deklarasi properti yang diterima oleh komponen Skeleton
interface Props {
  children?: JSX.Element;
  height: any;
  width: any;
  borderRadius?: number;
  marginBottom?: number;
  marginRight?: number;
}

// Membuat komponen Animation dengan animasi menggunakan Animated
const Animation = Animated.createAnimatedComponent(LinearGradient);

// Komponen utama Skeleton
const Skeleton = ({
  children,
  height,
  width,
  borderRadius,
  marginBottom,
  marginRight,
}: Props) => {
  // Membuat objek animatedValue sebagai pengendali animasi
  const animatedValue = new Animated.Value(0);

  // Efek animasi looping ketika komponen dimuat
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

  // Menghitung perubahan transformasi untuk efek bergerak
  const translateX = animatedValue.interpolate({
    inputRange: [0, 1],
    outputRange: [-width, width],
  });

  // Mengembalikan tampilan komponen Skeleton
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
};

// Gaya (style) untuk kontainer komponen Skeleton
const styles = StyleSheet.create({
  container: {
    backgroundColor: '#a0a0a0',
    overflow: 'hidden',
  },
});

export default Skeleton;
