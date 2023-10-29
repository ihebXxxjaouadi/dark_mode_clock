
import { useState, useRef, useEffect } from 'react';
import { Animated, Easing, View, Text, TouchableWithoutFeedback, Image, StyleSheet, StatusBar } from 'react-native'
import React from 'react'

export default function DarkModeView() {
    const [isDark, setDarkMode] = useState(false)
    const scale_color = useRef(new Animated.Value(0)).current
    const rotate = useRef(new Animated.Value(0)).current
    
    useEffect(() => {
         
    Animated.loop(Animated.timing(rotate, {
        duration: 10000,
        easing: Easing.linear,
        toValue:  1 ,
        useNativeDriver: false,
        
    })).start()

    }, [])
    return (
        <View style={{
            flex: 1,
            justifyContent: 'center',
            alignItems: 'center',
        }}>
            <StatusBar hidden />

            <View
                style={{
                    position: "absolute",
                    top: 30,
                    left: 30,
                }}>

                <View
                    style={{
                        alignItems: "center",
                        justifyContent: "center"
                    }}>
                    <Animated.View
                        style={{
                            backgroundColor: "#222",
                            width: 20,
                            height: 20,
                            transform: [
                                {
                                    scale: scale_color.interpolate(
                                        {
                                            inputRange: [0, 1],
                                            outputRange: [0, 100]
                                        }
                                    )
                                }
                            ],
                            borderRadius: 10,
                            position: "absolute"
                        }}></Animated.View>
                    <TouchableWithoutFeedback
                        onPress={() => {
                            setDarkMode(!isDark)
                            Animated.timing(scale_color, {
                                duration: 650,
                                easing: Easing.linear,
                                toValue: !isDark ? 1 : 0,
                                useNativeDriver: false
                            }).start()
                        }}

                    >
                        <Image
                            style={{
                                width: 35,
                                height: 35,

                            }}
                            source={!isDark ? require('../icons/moon.png') : require('../icons/sun.png')} />
                    </TouchableWithoutFeedback>

                </View>
            </View>
            <Animated.View style={{
                width: 180,
                height: 180,
                backgroundColor: scale_color.interpolate(
                    {
                        inputRange: [0, 1],
                        outputRange: ["#333", "#fff"]
                    }
                ),
                borderRadius: 180/2,
                shadowColor: scale_color.interpolate(
                    {
                        inputRange: [0, 1],
                        outputRange: ["#333", "#fff"]
                    }
                ),
                elevation: 30

            }}>
                <Animated.View style={{
                width: 180,
                height: 180,
                alignItems: "center",
                transform: [
                    {rotate: rotate.interpolate(
                        {
                            inputRange: [0, 1],
                            outputRange: ["0deg", "360deg"]
                        }
                    )}
                ]
                }}>
                    <Animated.View 
                    style={{
                        width: 13,
                        height: 68,
                        marginTop: 25,
                        backgroundColor:  scale_color.interpolate(
                            {
                                inputRange: [0, 1],
                                outputRange: ["#fff", "#333"]
                            }
                        ),
                        borderRadius: 100/2
                    }}/>
                </Animated.View>
            </Animated.View>
        </View>
    )
}

const styles = StyleSheet.create({
    clockView: {
        justifyContent: "center",
        alignItems: "center",
        flexDirection: "row",
        
    }
})
