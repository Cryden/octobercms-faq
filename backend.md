# Backend
Вопросы и ответы по backendу приложения и разработке плагинов

---

Как добавить свой формвиджет в Builder?

[Ответ](/backend/formwidgettobuilder.md)

---

Как получить список установленных плагинов?

Ответ:

```
        $plugins = \System\Models\PluginVersion::all()->lists('code'); // Вместо коде можно добавить свое поле отбора
 ```
