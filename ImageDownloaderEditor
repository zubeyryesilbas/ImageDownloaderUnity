using UnityEditor;
using UnityEngine;
using UnityEngine.Networking;
using System.Collections;
using System.IO;
using Unity.EditorCoroutines.Editor;

public class ImageDownloaderEditor : EditorWindow
{
    private string imageUrl = "https://example.com/image.jpg"; // Replace with your image URL
    private string savePath = "Assets/image.png"; // Replace with your save path

    [MenuItem("ImageDownloader")]
    public static void ShowWindow()
    {
        GetWindow<ImageDownloaderEditor>("Image Downloader");
    }

    void OnGUI()
    {
        GUILayout.Label("Download Image and Save as PNG", EditorStyles.boldLabel);

        imageUrl = EditorGUILayout.TextField("Image URL", imageUrl);
        savePath = EditorGUILayout.TextField("Save Path", savePath);

        if (GUILayout.Button("Download and Save"))
        {
            EditorCoroutineUtility.StartCoroutineOwnerless(DownloadAndSaveImage());
        }
    }

    private IEnumerator DownloadAndSaveImage()
    {
        using (UnityWebRequest webRequest = UnityWebRequestTexture.GetTexture(imageUrl))
        {
            yield return webRequest.SendWebRequest();

            if (webRequest.result != UnityWebRequest.Result.Success)
            {
                Debug.LogError("Failed to download image: " + webRequest.error);
            }
            else
            {
                Texture2D texture = DownloadHandlerTexture.GetContent(webRequest);
                SaveTextureAsPNG(texture, savePath);
            }
        }
    }

    private void SaveTextureAsPNG(Texture2D texture, string path)
    {
        byte[] bytes = texture.EncodeToPNG();
        File.WriteAllBytes(path, bytes);
        Debug.Log("Image saved to " + path);
        AssetDatabase.Refresh();
    }
}
