# translation for backend button in controller list_toolbar
```
<?= e(trans('backend::lang.form.create')) ?> 
```
====================
# showtree in config_list of controller 
```
# Show tree
showTree: true
treeExpanded: true
```
### RESULT
![result](https://raw.githubusercontent.com/idevmans/lpground/master/october-nested-tree.png)
====================
# download file with original name via routes lara-october
```
class download controll


use Illuminate\Support\Facades\Response;
use October\Rain\Support\Facades\File;
use Symfony\Component\HttpKernel\Exception\HttpException;

class DownloadController
{
    public function download($name): \Symfony\Component\HttpFoundation\Response
    {
        $filepath = temp_path('public/' . $name);

        if (!file_exists($filepath)) {
            throw new HttpException(404, 'File not found');
        }

        $headers = Response::download($filepath, $name)->headers->all();
        $response = Response::make(File::get($filepath), 200, $headers);

        @File::delete($filepath);
        return $response;
    }
}

route

Route::get('/download-file/{name}', '\Wms\Crm\Classes\DownloadController@download');
```
# Work with realtionships and rendering relation manager 
https://octobercms.com/docs/backend/relations#extend-view-widget

# Category for backend settings
https://github.com/octobercms/october/blob/develop/modules/system/classes/SettingsManager.php

# Merging config when you extend controller
```
Controller::extend(function($controller) {
            if (!in_array('Backend.Behaviors.RelationController',$controller->implement)) {
                $controller->implement[] = 'Backend.Behaviors.RelationController';
            }
            if (!isset($controller->relationConfig)) {
                $controller->addDynamicProperty('relationConfig');
            }
            $myConfigPath = '$/vendor/plugin/controllers/nameController/config_relation.yaml';


            $controller->relationConfig = $controller->mergeConfig(
                $controller->relationConfig,
                $myConfigPath
            );

        });
```
