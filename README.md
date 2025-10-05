import 'package:flutter/material.dart';


void main() => runApp(const MaterialApp(home: MarksheetApp(), debugShowCheckedModeBanner: false));


class Student {
  String name; int m1, m2, m3;
  Student(this.name, this.m1, this.m2, this.m3);
  int get total => m1 + m2 + m3;
  double get avg => total / 3;
}


class MarksheetApp extends StatefulWidget {
  const MarksheetApp({super.key});
  @override State<MarksheetApp> createState() => _MarksheetAppState();
}


class _MarksheetAppState extends State<MarksheetApp> {
  final sList = <Student>[], n = TextEditingController(), m1 = TextEditingController(), m2 = TextEditingController(), m3 = TextEditingController();


  void showForm([int? i]) {
    if (i != null) { n.text = sList[i].name; m1.text = '${sList[i].m1}'; m2.text = '${sList[i].m2}'; m3.text = '${sList[i].m3}'; }
    else { n.clear(); m1.clear(); m2.clear(); m3.clear(); }
    showDialog(context: context, builder: (_) => AlertDialog(
      title: Text(i == null ? 'Add Student' : 'Edit Student'),
      content: Column(mainAxisSize: MainAxisSize.min, children: [
        TextField(controller: n, decoration: const InputDecoration(labelText: 'Name')),
        TextField(controller: m1, decoration: const InputDecoration(labelText: 'Mark 1'), keyboardType: TextInputType.number),
        TextField(controller: m2, decoration: const InputDecoration(labelText: 'Mark 2'), keyboardType: TextInputType.number),
        TextField(controller: m3, decoration: const InputDecoration(labelText: 'Mark 3'), keyboardType: TextInputType.number),
      ]),
      actions: [TextButton(onPressed: () {
        final stu = Student(n.text, int.parse(m1.text), int.parse(m2.text), int.parse(m3.text));
        setState(() => i == null ? sList.add(stu) : sList[i] = stu); Navigator.pop(context);
      }, child: const Text('Save'))],
    ));
  }


  @override
  Widget build(BuildContext context) => Scaffold(
    appBar: AppBar(title: const Text('Student Marksheet')),
    body: ListView.builder(
      itemCount: sList.length,
      itemBuilder: (_, i) => ListTile(
        title: Text(sList[i].name),
        subtitle: Text('Total: ${sList[i].total}, Avg: ${sList[i].avg.toStringAsFixed(1)}'),
        trailing: Row(mainAxisSize: MainAxisSize.min, children: [
          IconButton(icon: const Icon(Icons.edit), onPressed: () => showForm(i)),
          IconButton(icon: const Icon(Icons.delete), onPressed: () => setState(() => sList.removeAt(i))),
        ]),
      ),
    ),
    floatingActionButton: FloatingActionButton(onPressed: () => showForm(), child: const Icon(Icons.add)),
  );
}
