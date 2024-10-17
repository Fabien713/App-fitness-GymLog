import React, { useState, useEffect } from 'react';
import { View, Text, StyleSheet, TouchableOpacity, Image, Modal, TextInput } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import Ionicons from 'react-native-vector-icons/Ionicons'; // For icons

// Create Bottom Tab Navigator
const Tab = createBottomTabNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Tab.Navigator
        screenOptions={({ route }) => ({
          tabBarIcon: ({ color, size }) => {
            let iconName;

            if (route.name === 'Profil') {
              iconName = 'person-outline';
            } else if (route.name === 'Historique') {
              iconName = 'time-outline';
            } else if (route.name === 'Entrainement') {
              iconName = 'add-circle-outline'; // Special icon for the middle tab
            } else if (route.name === 'Exercices') {
              iconName = 'barbell-outline';
            } else if (route.name === 'Tableau') {
              iconName = 'stats-chart-outline';
            } else if (route.name === 'Mesurer') {
              iconName = 'analytics-outline';
            } else if (route.name === 'Reseau') {
              iconName = 'people-outline';
            }

            // Return the icon component
            return <Ionicons name={iconName} size={size} color={color} />;
          },
          tabBarActiveTintColor: '#6200ee',
          tabBarInactiveTintColor: 'gray',
          tabBarStyle: {
            backgroundColor: '#121212', // Dark theme background for the tab bar
          },
        })}
      >
        <Tab.Screen name="Profil" component={ProfileScreen} />
        <Tab.Screen name="Historique" component={HistoryScreen} />
        <Tab.Screen name="Entrainement" component={TrainingScreen} />
        <Tab.Screen name="Exercices" component={ExercisesScreen} />
        <Tab.Screen name="Tableau" component={TableauScreen} />
        <Tab.Screen name="Mesurer" component={MeasureScreen} />
        <Tab.Screen name="Reseau" component={ReseauScreen} />
      </Tab.Navigator>
    </NavigationContainer>
  );
}

// Profile Screen Component
function ProfileScreen() {
  return (
    <View style={styles.container}>
      <View style={styles.profileSection}>
        <Image
          style={styles.avatar}
          source={{ uri: 'https://via.placeholder.com/150' }} // Replace with actual image link
        />
        <Text style={styles.username}>tailqr</Text>
        <Text style={styles.stats}>103 entraînements</Text>
      </View>
    </View>
  );
}

// History Screen Component
function HistoryScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.sectionTitle}>Historique</Text>
      <Text style={styles.placeholderText}>L'historique des entraînements apparaîtra ici.</Text>
    </View>
  );
}

// Training Screen Component
function TrainingScreen() {
  const [modalVisible, setModalVisible] = useState(false);
  const [workoutName, setWorkoutName] = useState('');
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    let interval;
    if (modalVisible) {
      interval = setInterval(() => {
        setSeconds(prevSeconds => prevSeconds + 1);
      }, 1000);
    }
    return () => clearInterval(interval);
  }, [modalVisible]);

  const handleCancelWorkout = () => {
    setModalVisible(false);
    setWorkoutName('');
    setSeconds(0);
  };

  return (
    <View style={styles.container}>
      <Text style={styles.sectionTitle}>Entrainement</Text>
      <TouchableOpacity style={styles.addButton} onPress={() => setModalVisible(true)}>
        <Text style={styles.addButtonText}>Commencer un nouvel entraînement</Text>
      </TouchableOpacity>

      {/* Modal for workout */}
      <Modal
        animationType="slide"
        transparent={true}
        visible={modalVisible}
        onRequestClose={handleCancelWorkout}
      >
        <View style={styles.modalContainer}>
          <View style={styles.modalContent}>
            <Text style={styles.modalTitle}>Insérez le nom de l'entraînement</Text>
            <TextInput
              style={styles.input}
              placeholder="Nom de l'entraînement"
              value={workoutName}
              onChangeText={setWorkoutName}
            />

            {/* Timer */}
            <Text style={styles.timerText}>Temps écoulé: {seconds} sec</Text>

            {/* Add exercise button */}
            <TouchableOpacity style={styles.addButton}>
              <Text style={styles.addButtonText}>Ajouter un exercice</Text>
            </TouchableOpacity>

            {/* Cancel workout button */}
            <TouchableOpacity style={styles.cancelButton} onPress={handleCancelWorkout}>
              <Text style={styles.cancelButtonText}>Annuler l'entraînement</Text>
            </TouchableOpacity>
          </View>
        </View>
      </Modal>
    </View>
  );
}

// Exercises Screen Component
function ExercisesScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.sectionTitle}>Exercices</Text>
      <Text style={styles.placeholderText}>Liste des exercices apparaîtra ici.</Text>
    </View>
  );
}

// Measure Screen Component
function MeasureScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.sectionTitle}>Mesurer</Text>
      <Text style={styles.placeholderText}>Vos mesures apparaîtront ici.</Text>
    </View>
  );
}

// Tableau Screen Component
function TableauScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.sectionTitle}>Tableau</Text>
      <Text style={styles.placeholderText}>Les statistiques apparaîtront ici.</Text>
    </View>
  );
}

// Reseau Screen Component
function ReseauScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.sectionTitle}>Reseau</Text>
      <Text style={styles.placeholderText}>Vos connexions apparaîtront ici.</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#121212', // Dark theme background
    padding: 20,
  },
  profileSection: {
    alignItems: 'center',
    marginBottom: 30,
  },
  avatar: {
    width: 80,
    height: 80,
    borderRadius: 40,
    marginBottom: 10,
  },
  username: {
    color: '#fff',
    fontSize: 18,
    fontWeight: 'bold',
  },
  stats: {
    color: '#888',
  },
  sectionTitle: {
    color: '#fff',
    fontSize: 16,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  addButton: {
    backgroundColor: '#6200ee',
    paddingVertical: 10,
    borderRadius: 5,
    alignItems: 'center',
  },
  addButtonText: {
    color: '#fff',
    fontSize: 16,
  },
  placeholderText: {
    color: '#888',
    textAlign: 'center',
    marginBottom: 10,
  },
  modalContainer: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: 'rgba(0, 0, 0, 0.8)',
  },
  modalContent: {
    backgroundColor: '#fff',
    padding: 20,
    borderRadius: 10,
    width: '80%',
    alignItems: 'center',
  },
  modalTitle: {
    fontSize: 18,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    padding: 10,
    borderRadius: 5,
    width: '100%',
    marginBottom: 10,
  },
  timerText: {
    fontSize: 16,
    marginBottom: 10,
  },
  cancelButton: {
    marginTop: 10,
  },
  cancelButtonText: {
    color: 'red',
    fontWeight: 'bold',
  },
});


