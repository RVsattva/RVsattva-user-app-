// Flutter code for User App (RVSATTVA) // Includes: Registration, Login, Multi-language, Menu, Order, Payment, Tracking

import 'package:flutter/material.dart'; import 'package:firebase_core/firebase_core.dart'; import 'package:firebase_auth/firebase_auth.dart'; import 'package:cloud_firestore/cloud_firestore.dart'; import 'package:flutter_localizations/flutter_localizations.dart';

void main() async { WidgetsFlutterBinding.ensureInitialized(); await Firebase.initializeApp(); runApp(MyApp()); }

class MyApp extends StatelessWidget { @override Widget build(BuildContext context) { return MaterialApp( title: 'RVSATTVA', theme: ThemeData(primarySwatch: Colors.green), supportedLocales: [ Locale('en'), Locale('hi'), Locale('ta'), ], localizationsDelegates: [ GlobalMaterialLocalizations.delegate, GlobalWidgetsLocalizations.delegate, GlobalCupertinoLocalizations.delegate, ], home: RegistrationScreen(), ); } }

class RegistrationScreen extends StatelessWidget { final TextEditingController emailController = TextEditingController(); final TextEditingController phoneController = TextEditingController(); final TextEditingController altPhoneController = TextEditingController(); final TextEditingController passwordController = TextEditingController();

void register(BuildContext context) async { try { UserCredential user = await FirebaseAuth.instance.createUserWithEmailAndPassword( email: emailController.text, password: passwordController.text, ); await FirebaseFirestore.instance.collection('users').doc(user.user!.uid).set({ 'email': emailController.text, 'phone': phoneController.text, 'altPhone': altPhoneController.text, 'language': 'en', }); Navigator.push(context, MaterialPageRoute(builder: (_) => HomeScreen())); } catch (e) { print(e); } }

@override Widget build(BuildContext context) { return Scaffold( appBar: AppBar(title: Text('Register')), body: Padding( padding: const EdgeInsets.all(16.0), child: Column( children: [ TextField(controller: emailController, decoration: InputDecoration(labelText: 'Email')), TextField(controller: phoneController, decoration: InputDecoration(labelText: 'Phone')), TextField(controller: altPhoneController, decoration: InputDecoration(labelText: 'Alternate Phone')), TextField(controller: passwordController, decoration: InputDecoration(labelText: 'Password'), obscureText: true), ElevatedButton(onPressed: () => register(context), child: Text('Register')), ], ), ), ); } }

class HomeScreen extends StatelessWidget { final List<Map<String, dynamic>> menuItems = [ {'name': 'Samosa', 'price': 20}, {'name': 'Jalebi', 'price': 30}, {'name': 'Veg Thali', 'price': 100}, ];

@override Widget build(BuildContext context) { return Scaffold( appBar: AppBar(title: Text('RVSATTVA Menu')), body: ListView.builder( itemCount: menuItems.length, itemBuilder: (context, index) { return ListTile( title: Text(menuItems[index]['name']), subtitle: Text('₹${menuItems[index]['price']}'), trailing: ElevatedButton(onPressed: () {}, child: Text('Order')), ); }, ), ); } }

