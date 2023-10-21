# Lr Folder Sync
This Lightroom plugin lets you synchonize folders and smart-collections with your online catalog, which is normally not possible. Hence this is a workaround with the goal to make this as easy, convenient, and reliable as possible.

In addition there comes a feature to remember the last selected photo(s) of a view (folder, collection, etc) and restoring those if you switch to this view later. A feature many times requested from Adobe, but they seem reluctant to implement this.

You can enable both features separately.

# Usage
## Auto-sync
### Select Folders and Smart-collections for Synchronization
To select a folder or smart-collection (sync-items) for synchronization you assign a configurable **color label** to it (default is green). I am fully aware that this prevents all users using the color labels for other purposes from effectively using this plugin. But this is a final decision and will not be changed upon request (with one exception: if there are enough requests to use the favorite flag, this could be implemented as alternative pretty simple).

### Rejected Photos
There are two options:
* Generally prevent rejected photos from syncing
* Use a different color (default: yellow) to mark sync-items to be synced without rejected photos

### Exclude Sub-folders
It is possible to use a third color (default: red) to exclude sync-items from being synced. This is of course only useful for sub-folders of folders marked to be synced. This also works for smart-collections within collection sets. However, while this works for folders over multiple hierarchy levels (i.e. you can exclude the sub-sub-sub-folder of a marked folder), this only works for smart-collections directly below a marked collection set.

### Synchronization
On a regular basis, or by triggering it manually (Library > Plugin-Extras > Sync Folders and Smart-collections) a collection will be created for each sync item within a special collection set (default: '_Auto-synced Folders'). The collections will have the same name as the sync-items with the path in paranthesis (this is necessary to prevent issues if two items in different locations have the same name). Earlier collections from sync-items, where the color label was removed, will be deleted. The collections will have the same photos as the sync-items. So if you delete or add photos to it, this will be reflected in the auto-sync collections.

Since this feature requires reading directly from the Lr database this adds some overhead regarding performance. Therefore it is strongly recommended to set the frequency not too low, i.e. 20 to 60 minutes is a very safe area (it was actually tested with 1 minute and did not cause issues, but still I would not recommend this).

And now the tricky part: Lr does not provide functions to programmatically set a collection to be synced with the online catalog. Therefore there are two workaround options:
* Manually: When new auto-sync collections have been created they are selected and you can activate syncing for all with just one click (on the checkbox to the left of any selected collection's name). This will cause a popup dialog, which gives you the choice to do this now or later. If you want to do this now your current view will change as the new collections are selected. This will also happen when the synchonization is started automatically in the background (you can disable this dialog, so it will never show again, although you will very likely forget to set the new collections to be synced).
* Automatically: In that case the new collections are set to be synced by the plugin. However, this requires directly writing to the Lr database! **IMPORTANT: This is of course not officially supported by Lightroom and if you choose to do so, you do this at your own risk!** Make sure to frequently backup the Lr database when using this feature. In addition this has some major drawbacks: Lr does not recognize these changes immediately. The new collections will not be shown as being synced and also WILL NOT sync right away. You nedd to **restart** Lightroom to reflect the changes. After restart the collections are shown as being synced and will start to sync immediately.

## Remember Selected Photos
This feature operates in the background. Whenever you change back to a view, the last selected photo(s) there will be selected again. This only works as long as Lr runs. Selections will not be remembered after restarting Lr.

# Installation
## Windows
Copy all files into the Lr plugin folder: %appdata%\Adobe\Lightroom\Modules

# Configuration


# Behind the Scenes
Currently the API of Lr does not allow to access or change any information in respect to online synchronization. Therefore the only option to provide such a functionality is to use the information directly from the Lightroom database (sqlite). This of course is not officially supported. You can use this plugin with only reading the database, which should not cause any issues or corrupt the database. However, please understand that I cannot guarantee this and you have to use this plugin at your own risk. So make backups of the database frequently. All I can say is that I never had any issues, even with using the features, which are writing into the database. Writing to the database is optional, but the only way to make this fully automatic. This will be explained later.

## AUto-sync
