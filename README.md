# stockapp
import java.net.URL;
import java.io.IOException;
import java.util.*;
import java.time.format.DateTimeFormatter;
import java.time.LocalDateTime;
import javax.swing.*;
import java.awt.*;
import java.text.DecimalFormat;

class Module{

    public static double ar[]=new double[30];
    public static int top=0;
    public static void add(double a){
        ar[top]=a;
        if(top<100)
            top+=1;
    }
    public static void pop(){
        for(int i=0;i<top-1;i++){
            ar[i]=ar[i+1];
        }
        top-=1;
    }
    public static void main(String... args) throws IOException {
        double cp=0,strp=0,lop=0,lcp=0,count=0;
        String str1="Therangebetweenthehighandlowpricesoverthepastday</div></span><divclass=\"P6K39c\">$";
        JFrame frame=new JFrame();
        frame.setVisible(true);
        frame.setBounds(80,10,1300,700);
        Container c=frame.getContentPane();
        c.setLayout(null);
        c.setBackground(Color.PINK);
        frame.setDefaultCloseOperation(3);

        JLabel label1=new JLabel("STOCK GUI");
        c.add(label1);
        label1.setBounds(550,0,600,50);
        Font font=new Font("Arial",Font.BOLD,40);
        label1.setFont(font);

        JLabel label2=new JLabel("Current Price     :  ");
        c.add(label2);
        label2.setBounds(1000,30,600,70);
        Font font1=new Font("Arial",Font.BOLD,20);
        label2.setFont(font1);

        JLabel label3=new JLabel("UA");
        c.add(label3);
        label3.setBounds(1190,30,600,70);
        Font font2=new Font("Arial",Font.BOLD,20);
        label3.setFont(font2);

        JLabel label4=new JLabel("Last Open Price:");
        c.add(label4);
        label4.setBounds(1000,50,600,70);
        label4.setFont(font1);

        JLabel label5=new JLabel("UA");
        c.add(label5);
        label5.setBounds(1190,50,600,70);
        label5.setFont(font2);

        JLabel label6=new JLabel("Last close Price:");
        c.add(label6);
        label6.setBounds(1000,70,600,70);
        label6.setFont(font1);

        JLabel label7=new JLabel("UA");
        c.add(label7);
        label7.setBounds(1190,70,600,70);
        label7.setFont(font2);




        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter format=DateTimeFormatter.ofPattern("\n\tdd-MM-yyyy\n\tHH:mm:ss:ms\t");


        try{

            //Instantiating the URL class
            URL url = new URL("https://www.google.com/finance/quote/KO:NYSE?hl=en");

            while(true){

                //Retriving content of the specified webpage

                Scanner sc = new Scanner(url.openStream());

                // Instantiating the StringBuffer class to hold the result
                StringBuffer sb = new StringBuffer();

                while(sc.hasNext()){
                    sb.append(sc.next());
                }
                //System.out.println(sb);
                try{
                    lop=Double.parseDouble(sb.substring(sb.indexOf(str1)+str1.length(),sb.indexOf(str1)+str1.length()+5));
                    label5.setText(String.valueOf(lop));}catch(Exception e){
                    e.printStackTrace();}
                try{
                    lcp=Double.parseDouble(sb.substring(sb.indexOf(str1)+str1.length()+7,sb.indexOf(str1)+str1.length()+12));
                    label7.setText(String.valueOf(lcp));}catch(Exception e){
                    e.printStackTrace();}
                try{
                    cp=Double.parseDouble(sb.substring(sb.indexOf("data-last-price=")+17,sb.indexOf("data-last-price=")+22));
                    label3.setText(String.valueOf(cp));
                    if(top>29)
                        pop();
                    add(cp);
                }catch(Exception e){
                    e.printStackTrace();}

                String formatDateTime=now.format(format);
                System.out.println("\n\tStock price at the time "+formatDateTime+" is :\t"+cp);
                System.out.println("The previous close and open is : "+lop+" and "+lcp);
                if(cp!=strp){
                    System.out.println("\n\t\tDismatch found \n\n");
                    strp=cp;}
                int jp=30;
                double dp=Math.min(lop,lcp);
                for(int j=0;j<13;j++){
                    DecimalFormat df=new DecimalFormat("##.##");
                    String s=df.format(dp);
                    JLabel label58=new JLabel(s);
                    Font fot=new Font("Aerial",Font.BOLD,20);
                    label58.setFont(fot);
                    c.add(label58);
                    label58.setBounds(80,jp+60,100,100);
                    jp+=40;
                    dp+=Math.abs(lop-lcp)/10;}
                int cx=0;
                for(int i=0;i<top;i++){
                    JLabel label59=new JLabel("*");
                    Font fot=new Font("Aerial",Font.BOLD,40);
                    label59.setFont(fot);
                    c.add(label59);
                    label59.setBounds(150+cx,(int)(565.0-(ar[i]-Math.min(lcp,lop))*400),100,100);
                    cx+=20;
                }
                cx=0;
            }}catch(Exception e){
            e.printStackTrace();}
    }
}  
