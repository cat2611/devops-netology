
Ответы  
1. Найдите полный хеш и комментарий коммита, хеш которого начинается на aefea.  
   Ответ:  
   команда git log |grep aefea  
   ```aefead2207ef7e2aa5dc81a34aedf0cad4c32545```  
 
2. Какому тегу соответствует коммит 85024d3?  
   Ответ:  
   команда git log --oneline --decorate -1 85024d3  
    ```85024d3100 (tag: v0.12.23) v0.12.23```
   
3. Сколько родителей у коммита b8d720? Напишите их хеши.  
   Ответ:  
   git cat-file -p b8d720 в выводе команды будут указаны родители  
   parent 56cd7859e05c36c06b56d013b55a252d0bb7e158    
   parent 9ea88f22fc6269854151c571162c5bcf958bee2b  
  
4. Перечислите хеши и комментарии всех коммитов, которые были сделаны между тегами v0.12.23 и v0.12.24.  
   Ответ:  
   Полные хеши коммитов можно посмотреть командой:  
   git log --pretty=format:"%H %s" v0.12.23..v0.12.24  
   ```33ff1c03bb960b332be3af2e333462dde88b279e v0.12.24  
   b14b74c4939dcab573326f4e3ee2a62e23e12f89 [Website] vmc provider links  
   3f235065b9347a758efadc92295b540ee0a5e26e Update CHANGELOG.md  
   6ae64e247b332925b872447e9ce869657281c2bf registry: Fix panic when server is unreachable  
   5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 website: Remove links to the getting started guide's old location  
   06275647e2b53d97d4f0a19a0fec11f6d69820b5 Update CHANGELOG.md  
   d5f9411f5108260320064349b757f55c09bc4b80 command: Fix bug when using terraform login on Windows  
   4b6d06cc5dcb78af637bbb19c198faff37a066ed Update CHANGELOG.md  
   dd01a35078f040ca984cdd349f18d0b67e486c35 Update CHANGELOG.md  
   225466bc3e5f35baa5d07197bbc079345b77525e Cleanup after v0.12.23 release  ```  
     
5. Найдите коммит, в котором была создана функция func providerSource, её определение в коде выглядит так: func providerSource(...) (вместо троеточия перечислены аргументы).  
   Ответ:  
   ищем в каких коммитах упоминается функция  
   git log -S "func providerSource" --oneline  
   5af1e6234a main: Honor explicit provider_installation CLI config when present  
   8c928e8358 main: Consult local directories as potential mirrors of providers  
   Убеждаемся что в коммите 8c928e8358(Git отображает самые старые коммиты снизу) есть создание функции:  
   ```git show 8c928e8358 |grep providerSource  
    +       providerSrc := providerSource(services)  
    +// providerSource constructs a provider source based on a combination of the  
    +func providerSource(services *disco.Disco) getproviders.Source {```  

6. Найдите все коммиты, в которых была изменена функция globalPluginDirs  
   Ответ:  
   Определим коммиты где упоминалась функция globalPluginDirs  
   git log -S "globalPluginDirs" --oneline --all  
   7c4aeac5f3 stacks: load credentials from config file on startup (#35952)  
   de49677ecd Run tf exec e2e tests  
   65c4ba7363 Remove terraform binary  
   aa3a155106 Remove accidentally-committed binary  
   e8a9debd2b Remove accidentally-committed binary  
   125eb51dc4 Remove accidentally-committed binary  
   e8eec68de3 backport of commit 1ee5d23894a0c9448d6787e0385dbba356db8096 (#30989)  
   b872613d25 Backport of Bump compatibility version to 1.3.0 for terraform core release into v1.2 (#30990)  
   22c121df86 Bump compatibility version to 1.3.0 for terraform core release (#30988)  
   fcdb5d2e55 (origin/f-plugin-finder) WIP centralized plugin finder  
   7c7e5d8f0a Don't show data while input if sensitive  
   35a058fb3d main: configure credentials from the CLI config file  
   c0b1761096 prevent log output during init  
   8364383c35 Push plugin discovery down into command package  

    Определим файлы в котором упомятнается функция globalPluginDirs    
    git log -S "globalPluginDirs" --name-only --oneline --all  
    7c4aeac5f3 stacks: load credentials from config file on startup (#35952)  
    commands.go  
    internal/command/cliconfig/plugins.go  
    de49677ecd Run tf exec e2e tests  
    terraform 2  
    65c4ba7363 Remove terraform binary  
    terraform  
    aa3a155106 Remove accidentally-committed binary  
    terraform  
    e8a9debd2b Remove accidentally-committed binary  
    terraform  
    125eb51dc4 Remove accidentally-committed binary  
    terraform  
    e8eec68de3 backport of commit 1ee5d23894a0c9448d6787e0385dbba356db8096 (#30989)  
    terraform  
    b872613d25 Backport of Bump compatibility version to 1.3.0 for terraform core release into v1.2 (#30990)  
    terraform  
    22c121df86 Bump compatibility version to 1.3.0 for terraform core release (#30988)  
    terraform  
    fcdb5d2e55 (origin/f-plugin-finder) WIP centralized plugin finder  
    plugins.go  
    7c7e5d8f0a Don't show data while input if sensitive  
    terraform  
    35a058fb3d main: configure credentials from the CLI config file  
    commands.go  
    c0b1761096 prevent log output during init  
    config_unix.go  
    8364383c35 Push plugin discovery down into command package  
    commands.go  
    plugins.go  


   Так как самый первый коммит затрагивает только два файла определим где эта функция создается   
   в файле commands.go функция только вызывается:  
   git show 8364383c35:commands.go |grep globalPluginDirs  
                GlobalPluginDirs: globalPluginDirs(),  
   В файле plugins.go функция создается    
   git show 8364383c35:plugins.go |grep globalPluginDirs  
    // globalPluginDirs returns directories that should be searched for  
    func globalPluginDirs() []string {s   
   проследим коммиты которые меняли эту функцию:  
   посмотрим что было сделано в коммите fcdb5d2e55    
     git show fcdb5d2e55:plugins.go  
     // globalPluginDirs returns directories that should be searched for  
    // globally-installed plugins (not specific to the current configuration).  
    //   
    // Earlier entries in this slice get priority over later when multiple copies  
    // of the same plugin version are found, but newer versions always override  
    // older versions where both satisfy the provider version constraints.  
    func globalPluginDirs() []string {  
            var ret []string  
            // Look in ~/.terraform.d/plugins/ , or its equivalent on non-UNIX  
            dir, err := cliconfig.ConfigDir()  
            if err != nil {  
                    log.Printf("[ERROR] Error finding global config directory: %s", err)  
            } else {  
                    machineDir := fmt.Sprintf("%s_%s", runtime.GOOS, runtime.GOARCH)  
                    ret = append(ret, filepath.Join(dir, "plugins"))  
                    ret = append(ret, filepath.Join(dir, "plugins", machineDir))  
            }   
            return ret  
         }    

Как видно из коммита там функция менялась.  
Проанализируем последний коммит 7c4aeac5f3  
 git show 7c4aeac5f3:plugins.go  
 func globalPluginDirs() []string {  
        var ret []string  
        // Look in ~/.terraform.d/plugins/ , or its equivalent on non-UNIX  
        dir, err := cliconfig.ConfigDir()  
        if err != nil {  
                log.Printf("[ERROR] Error finding global config directory: %s", err)  
        } else {  
                machineDir := fmt.Sprintf("%s_%s", runtime.GOOS, runtime.GOARCH)  
                ret = append(ret, filepath.Join(dir, "plugins"))  
                ret = append(ret, filepath.Join(dir, "plugins", machineDir))  
        }  
  
        return ret  
}  
тут она тоже присутсвует.  
Получается что функция меняется в коммитах 8364383c35 fcdb5d2e55 7c4aeac5f3  

8. Кто автор функции synchronizedWriters?  
   Ответ:  
   Определим первый коммит где упоминается synchronizedWriters  
    git log -S "func synchronizedWriters" --oneline --all  
     bdfea50cc8 remove unused  
     5ac311e2a9 main: synchronize writes to VT100-faker on Windows  
   Посмотрим кто автор:  
   git show 5ac311e2a9  
   commit 5ac311e2a91e381e2f52234668b49ba670aa0fe5  
    Author: Martin Atkins <mart@degeneration.co.uk>  
    Date:   Wed May 3 16:25:41 2017 -0700  


