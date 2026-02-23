# gestion-AHRIZ-ADERBAZ
import java.util.Scanner;
import java.io.*;

public class SystemedeGestiondeMagasain{
    static String[] noms = new String[100];
    static int[] quantites = new int[200];
    static double[] prix = new double[500];
    static int nbProduits = 0;
    static Scanner sc = new Scanner(System.in);
    public static void main(String[] args) {
        chargerStock();
        int option=0;
        do{
            afficherMenu();
            option = saisirEntier("Entrez votre option s'il vous plait");
            if(option==1){
                ajouterProduit();
            }
            else if(option==2){
                afficherStock();
            }
            else if(option==3){
                System.out.print("entrer le nom du produit que vous voulez chercher");
                String nom =sc.next();
                int j=rechercherProduit(nom);
                if(j!=-1){
                    System.out.println(noms[j]+":trouvé");
                }
                else {
                    System.out.println("le produit que vous chercher n'est pas en stock");
                }}

            else if(option==4){
                    effectuerVente();
                }
            else if(option==5){
                    etatAlerte();
                }
            else if(option==6){
                    sauvegarderStock();
                }
            else if(option==7){
                    System.out.println("sortir");
                }
            else{
                System.out.println("entrer un nombre entre 1 et 7");
            }
        }
        while(option!=7);
    }
    static void afficherMenu(){
        System.out.println("notre menu");
        System.out.println("1/Ajouter un nouveau produit");
        System.out.println("2/Inventaire");
        System.out.println("3/Recherche d'un produit");
        System.out.println("4/Realiser la vente");
        System.out.println("5/Alertes");
        System.out.println("6/Sauvegarder les donnees");
        System.out.println("7/Sortie");
    }
    static int saisirEntier(String message){
        System.out.print(message);
        int n=sc.nextInt();
        return n;
    }
    static void ajouterProduit(){
        if(nbProduits<100){
            System.out.println("Nom du produit:");
            noms[nbProduits]=sc.next();
            System.out.println("Prix du produit:");
            prix[nbProduits]=sc.nextDouble();
            quantites[nbProduits]=saisirEntier("Quantite du produit:");
            nbProduits++;
        } else {
            System.out.println("Le tableau est plein");
        }
    }
    static void modifierPrix() {
        System.out.print("Entrez le nom du produit que vous souhaiter de changer son prix : ");
        String nom = sc.next();
        int j=rechercherProduit(nom);
        if (j!=-1) {
            System.out.print("Nouveau prix : ");
            double nouveau = sc.nextDouble();
            prix[j] = nouveau;
        } else {
            System.out.println("Le produit que vous souhaitez de changer son prix est introuvable.");
        }
    }
    static int rechercherProduit(String nom) {
        for (int i = 0; i < nbProduits; i++) {
            if (noms[i].equalsIgnoreCase(nom)) {
                return i;
            }
        }
        return -1;
    }
    static void effectuerVente() {
        System.out.print("Nom du produit a vendre : ");
        String n = sc.next();
        int j=rechercherProduit(n);

        if (j==-1) {
            System.out.println("Le produit n'existe pas.");
        } else {
            int quantite = saisirEntier("Quantite demandee : ");
            if (quantites[j] >= quantite) {
                double total = prix[j] * quantite;
                if (total > 1000) {
                    System.out.println("Remise de 10%");
                    total = total * 0.9;
                }
                quantites[j]=quantites[j]-quantite;
                System.out.println("Ticket de caisse") ;
                System.out.println("L'article : " + noms[j]);
                System.out.println("Le prix d'unite : "+prix[j]+" dh");
                System.out.println("Votre total : " +total+" dh");
            } else {
                System.out.println("Le stock n'est pas suffisant" );
            }
        }
    }
    static void afficherStock() {
        System.out.println("NOM \t PRIX \t QUANTITE \t VALEUR");
        for (int i = 0; i < nbProduits; i++) {
            double valeurTotal = prix[i] * quantites[i];
            System.out.println(noms[i] + " \t " + prix[i] + " \t " + quantites[i] + " \t " + valeurTotal);
        }
    }
    static void etatAlerte() {
        for (int i = 0; i < nbProduits; i++) {
            if (quantites[i] < 5) {
                System.out.println("Alerte: "+noms[i]+" Stock du produit: " + quantites[i]);
            }
        }
    }
    static void sauvegarderStock() {
        try {
            PrintWriter writer = new PrintWriter(new FileWriter("stock.txt"));
            for (int i = 0; i < nbProduits; i++) {
                writer.println(noms[i] + ";" + prix[i] + ";" + quantites[i]);
            }
            writer.close();
            System.out.println("La sauvegarde est faite ");
        } catch (Exception e) {
            System.out.println("Erreur de sauvegarde.");
        }
    }
    static void chargerStock() {
        try {
            File f = new File("stock.txt");
            if (!f.exists()) return;
            Scanner lecteur = new Scanner(f);
            while (lecteur.hasNextLine()) {
                String ligne = lecteur.nextLine();
                String[] infos = ligne.split(";");
                noms[nbProduits] = infos[0];
                prix[nbProduits] = Double.parseDouble(infos[1]);
                quantites[nbProduits] = Integer.parseInt(infos[2]);
                nbProduits++;
            }
            lecteur.close();
        } catch (Exception e) {
            System.out.println("il ya un erreur de chargement.");
        }
    }
}

