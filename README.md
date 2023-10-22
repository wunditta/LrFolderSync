# Lr Folder Sync
This plugin offers three major features:
* Synchonize folders and smart-collections with your online catalog, which is normally not possible. Hence this is a workaround with the goal to make this as easy, convenient, and reliable as possible.
* Clean-up onliny synced photos which are not in synced collections anymore.
* Remember the last selected photo(s) of a view (folder, collection, etc) and restore them if you switch back to this view later.

You can enable the features separately.


# Usage
## Auto-sync
### Select Folders and Smart-collections for Synchronization
To select a folder or smart-collection (sync-items) for synchronization you assign a configurable **color label** to it (default is green). I am fully aware that this prevents all users using the color labels for other purposes from effectively using this plugin. But this is a final decision and will not be changed upon request (with one exception: if there are enough requests to use the favorite flag, this could be implemented as alternative pretty simple). Please note that you need one color in minimum to use this plugin, to use all features you will need three different colors.

### Rejected Photos
There are three options:
* Always sync rejected photos
* Generally prevent rejected photos from syncing
* Use a different color (default: yellow) to mark sync-items to be synced without rejected photos

### Exclude Sub-folders
It is possible to use a third color (default: red) to exclude sync-items from being synced. This is of course only useful for sub-folders of folders marked to be synced. This also works for smart-collections within marked collection sets. However, while this works for folders over multiple hierarchy levels (i.e. you can exclude the sub-sub-sub-folder of a marked folder), this only works for smart-collections directly below a marked collection set.

### Synchronization
On a regular basis, or by triggering it manually (Library > Plugin-Extras > Folder Sync > Sync Folders and Smart-collections) a collection will be created for each sync item within a special collection set (default: '_Auto-synced Folders'). The collections will have the same name as the sync-items with the path in paranthesis (this is necessary to prevent issues if two items in different locations have the same name). Earlier collections from sync-items, where the color label was removed, will be deleted. The collections will have the same photos as the sync-items. So if you delete or add photos to it, this will be reflected in the auto-sync collections.

Since this feature requires reading directly from the Lr database this adds some overhead regarding performance. Therefore it is strongly recommended to set the frequency not too low, i.e. 20 to 60 minutes is a very safe area (it was actually tested with 1 minute and did not cause issues, but still I would not recommend this).

And now the tricky part: Lr does not provide functions to programmatically set a collection to be synced with the online catalog. Therefore there are two workaround options:
* **Manually**: When new auto-sync collections have been created they are selected and you can activate syncing for all with just one click (on the checkbox to the left of any selected collection's name). This will cause a popup dialog, which gives you the choice to do this now or later. If you want to do this now your current view will change as the new collections are selected. This will also happen when the synchonization is started automatically in the background (you can disable this dialog, so it will never show again, although you will very likely forget to set the new collections to be synced).
* **Automatically**: In that case the new collections are set to be synced by the plugin. However, this requires directly writing to the Lr database! **IMPORTANT: This is of course not officially supported by Lightroom and if you choose to do so, you do this at your own risk!** Make sure to frequently backup the Lr database when using this feature. In addition this has some major drawbacks: Lr does not recognize these changes immediately. The new collections will not be shown as being synced and also WILL NOT sync right away. You nedd to **restart** Lightroom to reflect the changes. After restart the collections are shown as being synced and will start to sync immediately.

You can safely rename the 

## Clean-up
When you unsync a collection in Lr there are cases where the already synced photos will be kept in the online catalog, even so they are no longer in any synced collections. These leaves more and more orphaned photos online over time. Lr does not provide a function to clean-up those photos. This plugin offers such a feature.

In **Library > Plugin-Extras > Folder Sync > Clean-up** there are two functions to do so, although in most cases you will only need the first to clean-up the photos in 'All Synced Photographs'. The second is for cases where you have sync errors (because of missing photos) and want to clean-up those too.

This function looks for synced photos, which are not in any collections, which are set to be synced. If photos are found you have to manually remove them from 'All Synced Photograhps'. A popup dialog gives you two choices:
* Select: The orphaned photos are selected but shown together with all synced photos. This is useful if you just want to clean-up quickly. After confirming the dialog just press DEL key to remove the photos (nothing will be deleted, just removed from online catalog). A Lr warning dialog will show-up, which can be savely ignored as we only remove photos, which are no longer synced.
* Survey: The orphaned photos are shown in Survey view, so that you see only those photos. This is useful if you want to review the photos first before you remove them.

## Remember Selected Photos
The last selected photo(s) in each source (folder, collection, ...) is remembered and restored after coming back to this source.

This only works as long as Lr runs. Selections will not be remembered after restarting Lr.

The feature operates in the background. The plugin checks for changes every half second. The performance impact should still be neglectable. If not you can disable this feature.


# Installation
## Windows
Copy all files into the Lr plugin folder: %appdata%\Adobe\Lightroom\Modules

# Configuration
![](https://github.com/wunditta/Images/blob/main/LrFolderSync/SyncFolderConfig.png)

The options can be found in File > Plugin-Extras > Folder Sync > Options.

**Remember and restore selected photos for each folder**: Disable if you do not need this feature or you experience a performance impact.

**Enable Auto-Sync**: If disabled there is no automatic synchronizatiion of folders or smart-collections. You still can do this manually though.

**Auto-sync every \[minutes\]**: The frequency the synt-items are checked in the background automatically.

**Color to mark folders and smart-collections to be synced**: Choose a color for the color label you want to use to mark sync-items to be synced. Default is 'green'.

**Mark (sub-)folders and smart-collections to be excluded from syncing with color**: If enabled, sync-items marked with the chosen color (default is 'red') will be excluded from syncing. If disabled, a color label with this color will be ignored and can be used for other purposes.

**Never synchronize Rejected photos**: This generally prevent rejected photos from being synced. You can either enable this option, or the next, but not both.

**Mark folders and smart-collections to be synced without Rejected with color**: If enabled, sync-items marked with this color (default is 'yellow') will be synced but without rejected photos. Therefore you can include/exclude rejected photos on a per sync-item basis. Mark them with the standard color above (green) to be synced including rejected, or with this color to exclude them. If disabled, a color label with this color will be ignored and can be used for other purposes.


# Behind the Scenes
Currently the API of Lr does not allow to access or change any information in respect to online synchronization. Therefore the only option to provide such a functionality is to use the information directly from the Lightroom database (sqlite). This of course is not officially supported. You can use this plugin with only reading the database, which should not cause any issues or corrupt the database. However, please understand that I cannot guarantee this and you have to use this plugin at your own risk. So make backups of the database frequently. All I can say is that I never had any issues, even with using the features, which are writing into the database. Writing to the database is optional, but the only way to make this fully automatic. This will be explained later.

## AUto-sync
