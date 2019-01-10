# Как добавить свой формвиджет в Builder?

В файле Plugin.php добавляем 

```
public function boot()
    {
        Event::listen('pages.builder.registerControls', function ($controlLibrary) {

            function addTest($controlLibrary)
            {
                // Добавляем свойства

                $properties = [
                    'test' => [
                        'title' => 'Title',
                        'description' => 'Description',
                        'type' => 'string',
                    ],
                ];

                // Регистрируем формвиджет

                $controlLibrary->registerControl(
                    'test',
                    'test title',
                    'test description',
                    'default',
                    'icon-pencil-square',
                    $controlLibrary->getStandardProperties(['stretch'], $properties),
                    'CRYDEsigN\Socializer\FormWidgets\TestBuilder', // Указываем путь до своего формвиджета, в нашем случае это TestBuilder.php
                );
            }

            addTest($controlLibrary);
        });
    }

```

Коротко по коду. На событие Builder весим добавление своих формвиджетов. 

! Важно ! Формвиджет который будет отображаться в дальнейшем в формах, отличается от формвиджета в Builder.

Далее, создаем папку formwidgets, в ней TestBuilder.php

! Важно ! Не забудьте создать свой формвиджет Test.php, который будет отображаться в формах

```
<?php namespace Crydesign\Socializer\FormWidgets;

use RainLab\Builder\Widgets\DefaultControlDesignTimeProvider;
use Backend\Classes\WidgetBase;

class TestBuilder extends DefaultControlDesignTimeProvider
{
  protected $defaultControlsTypes = [
    'test',
  ];

  public function renderControlBody($type, $properties, $formBuilder)
  {
    if (!in_array($type, $this->defaultControlsTypes)) {
      return $this->renderUnknownControl($type, $properties);
    }

    return $this->makePartial($type, 
      [
        'properties'=>$properties,
        'formBuilder' => $formBuilder
      ]
    );
  }
}

?>
```

Данный код отвечает за вывод формвиджета в плагине Builder

! Важно ! Partial который будет выводится в Builder должен находиться в папке с именем formwidgets/testbuilder/partials/

Создаем файл _testbuilder.htm по пути указанному выше

```
<div class="builder-blueprint-control-text">
    <i class="icon-pencil-square"></i>Test Builder
</div>
```

Заходим в Builder, в списке доступных элементов формы должен появиться ваш формвиджет!

Использование данного примера можно посмотреть в плагине https://github.com/Cryden/oc-socializer-plugin