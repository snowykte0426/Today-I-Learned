# CNN 머신러닝 기반 인공지능 모델 제작
+ Python 3.12.2 기반으로 제작함
  ```python
    import os
  
    import keras
    import numpy as np
    import cv2
    from keras._tf_keras.keras.models import Sequential
    from keras._tf_keras.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout, BatchNormalization
    from keras._tf_keras.keras.utils import to_categorical
    from keras._tf_keras.keras.callbacks import EarlyStopping, ModelCheckpoint
    from sklearn.model_selection import train_test_split
    
    KNOWN_FACES_DIR = 'C:\\Faceon_Project\\DTFO_Taeeun\\known_faces'
    NON_FACES_DIR = 'C:\\Faceon_Project\\DTFO_Taeeun\\non_faces'
    
    def load_images_from_folder(folder, label):
        images = []
        labels = []
        for filename in os.listdir(folder):
            img_path = os.path.join(folder, filename)
            img = cv2.imread(img_path)
            if img is not None:
                img = cv2.resize(img, (128, 128))
                images.append(img)
                labels.append(label)
        return images, labels
    
    known_images = []
    known_labels = []
    label_mapping = {}
    registered_faces = [d for d in os.listdir(KNOWN_FACES_DIR) if d.isdigit()]
    
    for label, face_dir in enumerate(registered_faces):
        label_mapping[label] = face_dir
        face_path = os.path.join(KNOWN_FACES_DIR, face_dir)
        images, labels = load_images_from_folder(face_path, label)
        known_images.extend(images)
        known_labels.extend(labels)
    
    non_face_images, non_face_labels = load_images_from_folder(NON_FACES_DIR, len(registered_faces))
    
    all_images = known_images + non_face_images
    all_labels = known_labels + non_face_labels
    
    all_images = np.array(all_images)
    all_labels = to_categorical(np.array(all_labels), num_classes=len(registered_faces) + 1)
    
    X_train, X_test, y_train, y_test = train_test_split(all_images, all_labels, test_size=0.2, random_state=42)
    
    model = Sequential([
        Conv2D(32, (3, 3), activation='relu', input_shape=(128, 128, 3)),
        BatchNormalization(),
        MaxPooling2D((2, 2)),
        Conv2D(64, (3, 3), activation='relu'),
        BatchNormalization(),
        MaxPooling2D((2, 2)),
        Conv2D(128, (3, 3), activation='relu'),
        BatchNormalization(),
        MaxPooling2D((2, 2)),
        Conv2D(256, (3, 3), activation='relu'),
        BatchNormalization(),
        MaxPooling2D((2, 2)),
        Flatten(),
        Dense(512, activation='relu'),
        Dropout(0.5),
        Dense(len(registered_faces) + 1, activation='softmax')
    ])
    
    model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
    
    class TrainingProgressCallback(keras._tf_keras.keras.callbacks.Callback):
        def on_epoch_end(self, epoch, logs=None):
            print(f"Epoch {epoch + 1}/{10} - loss: {logs['loss']:.4f} - accuracy: {logs['accuracy']:.4f} - val_loss: {logs['val_loss']:.4f} - val_accuracy: {logs['val_accuracy']:.4f}")
    
    if os.path.exists('face_recognition_model.keras'):
        os.remove('face_recognition_model.keras')
    
    early_stopping = EarlyStopping(monitor='val_loss', patience=5, restore_best_weights=True)
    model_checkpoint = ModelCheckpoint('best_face_recognition_model.keras', save_best_only=True, monitor='val_loss')
    
    model.fit(X_train, y_train, epochs=10, validation_data=(X_test, y_test), callbacks=[TrainingProgressCallback(), early_stopping, model_checkpoint])
    
    model.save('face_recognition_model.keras')
    print("모델 저장 완료: face_recognition_model.keras")
    
    predictions = model.predict(X_test)
    predicted_labels = np.argmax(predictions, axis=1)
    true_labels = np.argmax(y_test, axis=1)
    
    print("테스트 셋에서의 정확도: ", np.mean(predicted_labels == true_labels))
    for i in range(10):
        true_label = label_mapping.get(true_labels[i], "Unknown")
        predicted_label = label_mapping.get(predicted_labels[i], "Unknown")
        print(f"실제 라벨: {true_label}, 예측 라벨: {predicted_label}")
    ```