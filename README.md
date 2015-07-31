## Android Annotations - Tutorial RSS

### O que é o Android Annotations?

O Android Annotations é um framework de código aberto que tem uma premissa simples: Uma dieta de
código. Ele faz todo o trabalho de implementar as coisas repetitivas por você, e lhe deixar concentrar
no que é realmente importante. Além disso, através da simplificação do seu código, a manutenção é
facilitada.

Um pequeno exemplo pode ser encontrado na [página principal](http://androidannotations.org/) do projeto.

Outro exemplo poderá ser encontrado neste tutorial, onde faremos uma pequena aplicação que recebe
um feed RSS auxiliados pelo Android Annotations.

### O que é preciso para este tutorial?

Neste tutorial estaremos usando o [Android Studio](https://developer.android.com/sdk/index.html), em
conjunto com o [Android Annotations](http://androidannotations.org/). ~~(D'oh)~~

Durante este tutorial também se assume algum conhecimento das API's do Android e de como usar o Android
Studio para programá-las. Então, caso não saiba nada disso ainda, talvez seja bom dar uma estudadinha antes,
o tutorial ~~deve~~ ficar por aqui enquanto isso. (:

### Obtendo o Android Annotations

Primeiramente, devemos obter o Android Annotations, e para isto teremos que usar o sistema de build
do Android Studio, que se chama Gradle. Para isto, após criar um novo projeto (Pode ser um com uma atividade
em branco), devemos modificar o arquivo *build.gradle* do módulo. Então, abra o arquivo build.gradles
que contém o nome Module ao seu lado e insira as seguintes linhas:

```gradle
//Dentro de campos repetidos, como repositories, apenas adicione o que há dentro as linhas
//já existentes no seu build.gradle.

buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {

        // Pode haver uma linha parecida com esta no seu arquivo,
        // mas use a que tiver a versão mais recente do gradle.
        classpath 'com.android.tools.build:gradle:1.2.3'

        // Usar a última versão do android-apt plugin. Ele serve para o gradle
        // poder baixar o android annotations da rede.
        classpath 'com.neenbedankt.gradle.plugins:android-apt:1.5.1'
    }
}

apply plugin: 'com.android.application'
apply plugin: 'com.neenbedankt.android-apt'

// Aqui é definida a versão que iremos usar do android annotations.
// Também pode ser modificada para a mais atual.
def AAVersion = '3.3.2'

dependencies {
    // Aqui se pede o download e processamento do plugin.
    apt "org.androidannotations:androidannotations:$AAVersion"
    compile "org.androidannotations:androidannotations-api:$AAVersion"
}

apt {
    arguments {
        androidManifestFile variant.outputs[0].processResources.manifestFile
    }
}
```

Uma vez configurado o gradle do seu projeto, já poderemos usar o Annotations em nosso projeto.

### Mas e a aplicação RSS?

Calma, já já vamos começar a fazê-la, mas para isto vamos explorando primeiro como obter algum resultado
a partir de um link rss. Para isto, vamos começar de um link hardcoded numa atividade vazia, apenas
para ter uma ideia de como as coisas funcionam. Podemos notar que assim que uma atividade nova é
criada no Android Studio, nos deparamos com um método sempre igual chamado onCreate que apenas seta
o layout da classe. Com o Android Annotations, podemos nos livrar deste método apenas adicionando
@EActivity no inicio de nossa classe. Suponha que nós temos uma main activity, então podemos nos livrar
do onCreate e ficar com:

```java
@EActivity(R.layout.activity_main)
public class MainActivity extends Activity {

    @Override
        public boolean onCreateOptionsMenu(Menu menu) {
            getMenuInflater().inflate(R.menu.menu_main, menu);
            return true;
        }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();

        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }

}
```

Porém, ainda temos dois métodos com algum código dentro que podem vir a ser usados mais tarde. Bem,
como você já deve imaginar, também existem annotations para isto, e até mesmo para o clique nas
settings. O que nos deixaria com o seguinte código:

```java
@EActivity(R.layout.activity_main)
@OptionsMenu(R.menu.menu_main)
public class MainActivity extends Activity {

    @OptionsItem(R.id.action_settings)
    void settingsSelected(){
        //Qualquer código aqui.
    }

}
```

Uma pequena redução de código e um pequeno aumento na legibilidade, não? (: